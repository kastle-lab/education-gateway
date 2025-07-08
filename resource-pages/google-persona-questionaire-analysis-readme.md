## Understanding the Flow of the Google Apps Script

This script uses Google Form submissions, calculates persona scores based on predefined mappings, updates a Google Sheet, and optionally sends an email with the results. Below is a step-by-step breakdown of the script's execution flow.

### Flow of Execution

* Triggered Function: onFormSubmit(e)
* Automatically triggered when a Google Form is submitted via the Google Apps Script trigger.
* Fetches the target Google Sheet using its ID.
* Logs details of the sheet and event data for debugging.
* Validates if the form response exists; exits early if no response is received.
* Extracts user responses and trims unnecessary spaces.
* Determines the row where the submission is recorded. – latest = last row
* Initializes persona scores to zero for each persona category.
* Retrieves persona mappings and weight distributions from the "PersonaMappings" sheet and computes persona scores based on         weighted mappings using the helper function defined.
* Sorts and formats the scores in descending order using a helper function.
* Updates the Google Sheet with the calculated persona scores using a helper function.
* (Optional) Sends an email notification if enabled.

#### Helper Functions
 
 * initializePersonaScores()
* Initializes an object with all personas set to a score of 0.
* getValidatedPersonaMappingsFromSheet()
* Retrieves persona weight mappings from the "PersonaMappings" sheet.
* Parses JSON-formatted weight values to create an answer-to-persona mapping.
* Logs errors if JSON parsing fails or if invalid formats are encountered.
* calculateWeightedPersonaScores(personaScores, answers, personaMappings)
* Iterates through user answers and checks if they exist in the persona mappings.
* If a mapping exists, updates the corresponding persona scores using predefined weights.
* Logs details for debugging and warns about unmapped answers.
* formatScores(personaScores)
* Filters out personas with a score of 0.
* Sorts personas in descending order of scores.
* Formats the output as a comma-separated string.
* updateSheet(sheet, row, formattedScores)
vUpdates the response sheet by inserting the formatted persona scores into a specific row and column.
* Logs confirmation of a successful update.
    * Email Notification (Optional)
* The script includes an optional email notification feature.
* If emailFlag is set to true, an email is sent to the user with their persona results.
* The email contains both the top persona and a list of all calculated scores.

### How to Use This Script?

* Deploy the Google Apps Script in your Google Sheet.
* Set up a trigger to run onFormSubmit(e) whenever a Google Form is submitted.
* Ensure that the "PersonaMappings" sheet contains valid mappings in JSON format.
* (Optional) Enable the email feature by setting emailFlag = true.
* Check the Google Sheet for persona results after form submissions.
