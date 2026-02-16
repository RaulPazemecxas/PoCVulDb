  ┌─────────┬───────────────────┐
  │  Fields │  Anwsers          │
  ├─────────┼───────────────────┤
  │ Vendor  │ Linux             │
  ├─────────┼───────────────────┤
  │ Product │ Linux Kernel      │
  ├─────────┼───────────────────┤
  │ Version │ 6.14 through 6.19 │
  ├─────────┼───────────────────┤
  │ Class   │ Memory Corruption │
  └─────────┴───────────────────┘



  A dangling pointer vulnerability exists in the Linux kernel's DAMON
  subsystem in mm/damon/core.c, function damos_walk(). When called on
  an inactive DAMON context, the function sets ctx->walk_control to a
  stack-allocated structure (line 1560) and returns -EINVAL (line 1563)
  without clearing the pointer. This leaves ctx->walk_control pointing
  to freed stack memory containing a function pointer (walk_fn).

  This causes a permanent denial of service (all subsequent
  damos_walk() calls return -EBUSY) and leaves a dangling pointer
  that could lead to use-after-free with arbitrary code execution
  if dereferenced.

  The bug is structurally identical to CVE-2026-23012, which was the
  same pattern in the sibling function damon_call(), fixed in commit
  f9132fbc2e83.

  Introduced in commit bf0eaba0ff9c ("mm/damon/core: implement
  damos_walk()"). Affects Linux kernel v6.14 through v6.19.
  Confirmed and reproduced on 6.19.0 via DAMON sysfs interface.


# Reproduction (confirmed on 6.19.0, CONFIG_DAMON=y CONFIG_DAMON_SYSFS=y)

  DAMON=/sys/kernel/mm/damon/admin/kdamonds

  # Setup context with scheme
  echo 1 > $DAMON/nr_kdamonds
  echo 1 > $DAMON/0/contexts/nr_contexts
  echo vaddr > $DAMON/0/contexts/0/operations
  echo 1 > $DAMON/0/contexts/0/targets/nr_targets
  echo $$ > $DAMON/0/contexts/0/targets/0/pid_target
  echo 1 > $DAMON/0/contexts/0/schemes/nr_schemes
  echo stat > $DAMON/0/contexts/0/schemes/0/action

  # Start then stop (ctx stays allocated per sysfs design)
  echo on > $DAMON/0/state
  sleep 1
  echo off > $DAMON/0/state
  sleep 1

  # Trigger bug: damos_walk() on inactive context
  echo "update_schemes_tried_regions" > $DAMON/0/state
  # Returns -EINVAL, walk_control left dangling

  # Confirm: second call gets -EBUSY (dangling pointer != NULL)
  echo "update_schemes_tried_regions" > $DAMON/0/state
  # Returns -EBUSY -- interface permanently locked

## Tested output

  First call:  -EINVAL (Invalid argument)
  Second call: -EBUSY (Device or resource busy) <-- BUG confirmed

## Root cause

Commit bf0eaba0ff9c ("mm/damon/core: implement damos_walk()")
introduced this function without cleanup on the -EINVAL error path.

The sibling function damon_call() had the exact same bug and was
fixed in f9132fbc2e83 by adding damon_call_handle_inactive_ctx()
which removes the control object when the context is inactive.
damos_walk() has no equivalent cleanup.

## Impact

1. PERMANENT LOCKUP: After on->off->update_schemes_tried_regions,
   all future tried_regions queries return -EBUSY forever until
   the DAMON context is destroyed.

2. DANGLING POINTER: ctx->walk_control points to freed stack memory.
   The struct damos_walk_control contains a function pointer
   (walk_fn). If any DAMON API consumer reuses the same ctx after
   damos_walk() returns -EINVAL and kdamond is restarted, it would
   dereference the dangling pointer in damos_walk_call_walk()
   (which calls control->walk_fn) or damos_walk_cancel().


