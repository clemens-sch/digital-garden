#Software-Engineering 

[Main-Source](https://deep-thought.norwin.at/tech-kb/web-development/exercises/Exercise-Recipe-Collection/)

---
## # Exercise Recipe Collection

Create an application for managing recipes. Recipes can be categorized. A recipe can belong to zero, one, or multiple categories. For example, the same dish could be categorized under both "Diet Recipes" and "Vegetables".

The following properties should be stored for recipes and categories. The IDs are generated surrogate keys.

Recipe:

- Recipe ID
- Title
- Description
- Ingredients
- Instructions
- Cooking Time
- Difficulty Level

Category:

- Category ID
- Name
- Description

### # 1. Project Structure

Divide your solution into at least 4 projects: Model, Domain, Api, Web. You can add more projects if necessary or sensible.

- Model:
    - Entity Framework Db Context
    - Entities
- Domain:
    - Repositories
- API:
    - Asp.Net Core Controllers (REST)
- Web:
    - Blazor Client

### # 2. Database Model

Create the DbContext and all necessary classes for entities. Create a database and ensure all required tables exist.

### # 3. Repositories

Use the IRepository interface, create a base class RepositoryBase, and implement the repositories for Recipe and Category.

### # 4. API

Create a REST API using ASP.NET Core Controller. Use repositories exclusively for accessing the database, and not directly the database context. Implement a generic controller, and derive from it a RecipeController and a CategoryController.

### # 5. Web Client

Create a Blazor web client that supports the following functions:

- Display a list of recipes
    - Click on a recipe -> Navigate to recipe detail view
- Display a list of categories
    - Click on a category -> Display a list of recipes in that category
    - It should be possible to remove the selected category -> Return to the list of categories
    - The selected category should be saved by the application. When navigating to the category view next time, the last selected category should still be active, and its associated recipes should be displayed, regardless of which page was displayed previously.
- Create and delete recipes and categories
- Associate recipes with categories, or dissolve associations
- Search function (input in a text field)
    - Result: Display of matching recipes
    - Searched fields: Title, Description, Category

---
## # Implementation

### # 1. Project Structure

