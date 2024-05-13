# Programming-POE-Part-2

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;



class Ingredient
{
    public string Name { get; set; }
    public double Quantity { get; set; }
    public string Unit { get; set; }
    public double OriginalQuantity { get; set; } 
    public int Calories { get; set; }
    public string FoodGroup { get; set; }

   
    public Ingredient(string name, double quantity, string unit, int calories, string foodGroup)
    {
        Name = name;
        Quantity = quantity;
        Unit = unit;
        OriginalQuantity = quantity; 
        Calories = calories;
        FoodGroup = foodGroup;
    }

    
    public void Scale(double factor)
    {
        Quantity *= factor;
    }

   
    public void Reset()
    {
        Quantity = OriginalQuantity;
    }
}


class Step
{
    public string Description { get; set; }

    
    public Step(string description)
    {
        Description = description;
    }
}

// recipe class
class Recipe
{
    public string Name { get; set; }
    public List<Ingredient> Ingredients { get; set; }
    public List<Step> Steps { get; set; }

    // Constructor
    public Recipe(string name)
    {
        Name = name;
        Ingredients = new List<Ingredient>();
        Steps = new List<Step>();
    }

    // add ingredients
    public void AddIngredient(string name, double quantity, string unit, int calories, string foodGroup)
    {
        Ingredients.Add(new Ingredient(name, quantity, unit, calories, foodGroup));
    }

    
    public void AddStep(string description)
    {
        Steps.Add(new Step(description));
    }

   
    public void ScaleIngredients(double factor)
    {
        foreach (Ingredient ingredient in Ingredients)
        {
            ingredient.Scale(factor);
        }
    }

    
    public void ResetQuantities()
    {
        foreach (Ingredient ingredient in Ingredients)
        {
            ingredient.Reset();
        }
    }

    // display details
    public void DisplayRecipe()
    {
        Console.WriteLine("Recipe: " + Name);
        Console.WriteLine("Ingredients:");
        foreach (Ingredient ingredient in Ingredients)
        {
            Console.WriteLine($"{ingredient.Quantity} {ingredient.Unit} of {ingredient.Name}");
        }

        Console.WriteLine("\nSteps:");
        for (int i = 0; i < Steps.Count; i++)
        {
            Console.WriteLine($"{i + 1}. {Steps[i].Description}");
        }
    }

    // calculate calories
    public int CalculateTotalCalories()
    {
        return Ingredients.Sum(ingredient => ingredient.Calories);
    }
}


class Program
{
    static List<Recipe> recipes = new List<Recipe>();

    static void Main()
    {
        bool exit = false;
        while (!exit)
        {
            Console.WriteLine("\n1. Enter a new recipe");
            Console.WriteLine("2. Display all recipes");
            Console.WriteLine("3. Exit");
            Console.Write("Choose an option: ");
            string option = Console.ReadLine();

            switch (option)
            {
                case "1":
                    EnterNewRecipe();
                    break;
                case "2":
                    DisplayAllRecipes();
                    break;
                case "3":
                    exit = true;
                    break;
                default:
                    Console.WriteLine("Invalid option.");
                    break;
            }
        }
    }

    static void EnterNewRecipe()
    {
        Console.Write("\nEnter recipe name: ");
        string name = Console.ReadLine();
        Recipe recipe = new Recipe(name);

        Console.Write("Enter the number of ingredients: ");
        int numIngredients = int.Parse(Console.ReadLine());
        for (int i = 0; i < numIngredients; i++)
        {
            Console.WriteLine($"Enter details for ingredient {i + 1}:");
            Console.Write("Name: ");
            string ingredientName = Console.ReadLine();
            Console.Write("Quantity: ");
            double quantity = double.Parse(Console.ReadLine());
            Console.Write("Unit: ");
            string unit = Console.ReadLine();
            Console.Write("Calories: ");
            int calories = int.Parse(Console.ReadLine());
            Console.Write("Food Group: ");
            string foodGroup = Console.ReadLine();

            recipe.AddIngredient(ingredientName, quantity, unit, calories, foodGroup);
        }

        Console.Write("Enter the number of steps: ");
        int numSteps = int.Parse(Console.ReadLine());
        for (int i = 0; i < numSteps; i++)
        {
            Console.WriteLine($"Enter step {i + 1}:");
            string description = Console.ReadLine();
            recipe.AddStep(description);
        }

        recipes.Add(recipe);
        Console.WriteLine("\nRecipe added successfully!");
    }

    static void DisplayAllRecipes()
    {
        if (recipes.Count == 0)
        {
            Console.WriteLine("\nNo recipes available.");
            return;
        }

        Console.WriteLine("\nList of Recipes:");
        foreach (Recipe recipe in recipes.OrderBy(r => r.Name))
        {
            Console.WriteLine($"- {recipe.Name}");
        }

        Console.Write("\nEnter the name of the recipe to display details: ");
        string recipeName = Console.ReadLine();
        Recipe selectedRecipe = recipes.FirstOrDefault(r => r.Name.Equals(recipeName, StringComparison.OrdinalIgnoreCase));
        if (selectedRecipe != null)
        {
            selectedRecipe.DisplayRecipe();
            int totalCalories = selectedRecipe.CalculateTotalCalories();
            Console.WriteLine($"\nTotal Calories: {totalCalories}");
            if (totalCalories > 300)
            {
                Console.WriteLine("Warning: Total calories exceed 300!");
            }
        }
        else
        {
            Console.WriteLine("Recipe not found.");
        }
    }
}


