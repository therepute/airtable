# Airtable Scripting Examples

This repository contains example scripts for interacting with an Airtable base using the Scripting app or Airtable Automations.

## Prerequisites

- You must have access to an Airtable base.
- The code should be run in the Airtable Scripting app, or in an Automation script step.

## Adding Fields to an Airtable Table

The script below will:
1. Access the table named `"NE"`.
2. Check if each specified field exists.
3. If a field does not exist, the script will create it.

**Note:**  
- Make sure to replace `"NE"` with the actual name of your table.
- Update the `fieldsToAdd` array with the fields you wish to create.

```js
async function createFields() {
    try {
        const tableName = "NE"; // change this if your table name is different
        const table = base.getTable(tableName);
        
        // Define the fields you want to add
        const fieldsToAdd = [
            { name: "New_Field_1", type: "number" },
            { name: "New_Field_2", type: "singleLineText" },
            { name: "Another_Field", type: "multilineText" }
        ];

        for (const field of fieldsToAdd) {
            // Check if the field already exists
            const existingField = table.fields.find(f => f.name === field.name);
            if (!existingField) {
                // Create the field
                await table.createFieldAsync(field.name, field.type);
                console.log(`Created field: ${field.name} (${field.type})`);
                // Optional: small delay between creations to avoid rate limits
                await new Promise(resolve => setTimeout(resolve, 100));
            } else {
                console.log(`Field '${field.name}' already exists. Skipping.`);
            }
        }

        console.log("Done creating fields!");

    } catch (error) {
        console.error("Error creating fields:", error);
    }
}

await createFields();



**Listing Fields in an Airtable Table**


Copy code
async function listTableFields() {
    const table = base.getTable("NE"); // Replace if needed
    const fields = table.fields.map(field => field.name);

    console.log("Fields in 'NE' table:");
    for (const fieldName of fields) {
        console.log(fieldName);
    }
}

await listTableFields();

