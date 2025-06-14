📌 Stored Cross-Site Scripting (XSS) Vulnerability in My Food Recipe (SourceCodester)

A Stored Cross-Site Scripting (XSS) vulnerability was discovered in the My Food Recipe application developed by SourceCodester. The vulnerability resides in the "Add Recipe" functionality, where user-supplied input is improperly sanitized before being stored and rendered, allowing arbitrary JavaScript to be executed in the context of other users' sessions.

Vulnerable Component

**Affected Field: recipe_name (input type: text)**

**Affected Endpoint: /endpoint/add-recipe.php (via #addRecipeModal modal form)**

**Input Vector: POST request via form submission**

**Impact: Stored JavaScript payload is triggered when the page loads.**

Steps to Reproduce


1 - Open the modal by clicking the Add Recipe button:

![image](https://github.com/user-attachments/assets/63355914-dcb3-4f89-bf44-bfcfc34cddde)

<button type="button" class="btn btn-add-food btn-secondary" data-toggle="modal" data-target="#addRecipeModal">Add Recipe</button>

![image](https://github.com/user-attachments/assets/46baf795-cd9c-453a-801a-7b33d3e4ee23)
![image](https://github.com/user-attachments/assets/401b0283-22b8-4a54-8b38-6b2001ae643f)

2 - In the Recipe Name field, insert the following payload:

![image](https://github.com/user-attachments/assets/2c011ce2-3259-43a5-9925-aaf20ec7ab2c)

<script>alert('PoC VulDB My Food Recipe')</script>

3 - Fill the remaining fields with valid data (e.g., category, ingredients, procedure) and click Save changes.

Upon submitting the form, the payload is stored in the database.

Whenever the recipe data is rendered again (e.g., recipe listing or detail views), the JavaScript is executed, confirming a persistent (stored) XSS vulnerability.

![image](https://github.com/user-attachments/assets/71a16a62-ece7-4ce9-8090-b0386f01499e)
![image](https://github.com/user-attachments/assets/81226380-1164-4ff0-9d80-f9c8aefeaefc)

