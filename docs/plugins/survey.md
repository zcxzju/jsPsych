# survey

The survey displays one or more questions of different types, on one or more pages that the participant can navigate. 
The supported question types are: "drop-down", "likert", "multi-choice", "multi-select", "ranking", "rating", "text". 
There is also an "html" type for adding arbitrary HTML-formatted content (without any associated response field) in the question set.

## Parameters

In addition to the [parameters available in all plugins](../overview/plugins.md#parameters-available-in-all-plugins), this plugin accepts the following parameters. 
Parameters with a default value of *undefined* must be specified. 
Other parameters can be left unspecified if the default value is acceptable.

### Survey parameters

Parameter | Type | Default Value | Description
----------|------|---------------|------------
pages | array | *undefined* | An array of arrays. Each inner array contains the content for a single page, which is made up of one or more question objects. 
randomize_question_order | boolean | `false` | If `true`, the display order of the questions within each survey page is randomly determined at the start of the trial. In the data object, if question `name` values are not provided, then `Q0` will refer to the first question in the array, regardless of where it was presented visually.
button_label_next | string |  'Continue' | Label of the button to move forward to the next page, or finish the survey.
button_label_previous | string | 'Back' | Label of the button to move to a previous page in the survey.
button_label_finish | string | 'Finish' | Label of the button to submit responses.
autocomplete | boolean | `false` | This determines whether or not all of the input elements on the page should allow autocomplete. Setting this to `true` will enable autocomplete or auto-fill for the form.
show_question_numbers | string | "off" | One of: "on", "onPage", "off". If "on", questions will be labelled starting with "1." on the first page, and numbering will continue across pages. If "onPage", questions will be labelled starting with "1.", with separate numbering on each page. If "off", no numbers will be added before the question prompts. Note: HTML question types are ignored in automatic numbering.
title | string | `null` | If specified, this text will be shown at the top of the survey pages. This parameter provides a method for fixing any arbitrary text to the top of the page when randomizing the question order with the `randomize_question_order` parameter (since HTML question types are also randomized).

### Question types and parameters

#### Parameters for all question types

Parameters with a default value of *undefined* must be specified. 
Other parameters can be left unspecified if the default value is acceptable.

Parameter | Type | Default Value | Description
----------|------|---------------|------------
type | string | *undefined* | The question type. Options are: "drop-down", "html", "likert", "multi-choice", "multi-select", "ranking", "rating", "text".
prompt | string | *undefined* | The prompt/question that will be associated with the question's response field. If the question type is "html", then this string is the HTML-formatted string to be displayed. 
required | boolean | `false` | Whether a response to the question is required (`true`) or not (`false`), using the HTML5 `required` attribute. 
name | string | `null` | Name of the question to be used for storing data. If this parameter is not provided, then default names will be used to identify the questions in the data: `Q0`, `Q1`, etc.

#### HTML

Present arbitrary HTML-formatted content in the list of questions, including text, images, and sounds. There are no response options.

The only available parameters are those listed for all question types. 
Parameters with a default value of *undefined* must be specified.
Other parameters can be left unspecified if the default value is acceptable.

#### Multi-choice

Present a question with a limited set of options. The participant can only select one option.

In addition to the parameters for all question types, the multi-choice question type also offers the following parameters. 
Parameters with a default value of *undefined* must be specified.
Other parameters can be left unspecified if the default value is acceptable.

Parameter | Type | Default Value | Description
----------|------|---------------|------------
options | array of strings | *undefined* | This array contains the set of multiple choice options to display for the question.
option_reorder | string | "none" | One of: "none", "asc", "desc", "random". If "none", the options will be listed in the order given in the `options` array. If `random`, the option order will be randomized. If "asc" or "desc", the options will be presented in ascending or descending order, respectively.
columns | integer | 1 | Number of columns to use for displaying the options. If 1 (default), the choices will be displayed in a single column (vertically). If 0, choices will be displayed in a single row (horizontally). Any value greater than 1 can be used to display options in multiple columns. 

#### Multi-select

Present a question with a limited set of options. The participant can select multiple options.

In addition to the parameters for all question types, the multi-choice question type also offers the following parameters. 
Parameters with a default value of *undefined* must be specified.
Other parameters can be left unspecified if the default value is acceptable.

Parameter | Type | Default Value | Description
----------|------|---------------|------------
options | array of strings | *undefined* | This array contains the set of options to display for the question.
option_reorder | string | "none" | One of: "none", "asc", "desc", "random". If "none", the options will be listed in the order given in the `options` array. If `random`, the option order will be randomized. If "asc" or "desc", the options will be presented in ascending or descending order, respectively.
columns | integer | 1 | Number of columns to use for displaying the options. If 1 (default), the choices will be displayed in a single column (vertically). If 0, choices will be displayed in a single row (horizontally). Any value greater than 1 can be used to display options in multiple columns. 

#### Text

Present a question with a free response text field in which the participant can type in an answer.

In addition to the parameters for all question types, the text question also offers the following parameters. 
Parameters with a default value of *undefined* must be specified. 
Other parameters can be left unspecified if the default value is acceptable.

Parameter | Type | Default Value | Description
----------|------|---------------|------------
placeholder | string | "" | Placeholder text in the text response field. 
textbox_rows | integer | 1 | The number of rows (height) for the response text box. 
textbox_columns | integer | 40 | The number of columns (width) for the response text box. 
validation | string | "" | A regular expression used to validate the response.

## Data Generated

In addition to the [default data collected by all plugins](../overview/plugins.md#data-collected-by-all-plugins), this plugin collects the following data for each trial.

Name | Type | Value
-----|------|------
response | object | An object containing the response for each question. The object will have a separate key (variable) for each question, with the first question in the trial being recorded in `Q0`, the second in `Q1`, and so on. The responses are recorded as the name of the option label selected (string). If the `name` parameter is defined for the question, then the response object will use the value of `name` as the key for each question. This will be encoded as a JSON string when data is saved using the `.json()` or `.csv()` functions. |
rt | numeric | The response time in milliseconds for the subject to make a response. The time is measured from when the questions first appear on the screen until the subject's response(s) are submitted. |
question_order | array | An array with the order of questions. For example `[2,0,1]` would indicate that the first question was `trial.questions[2]` (the third item in the `questions` parameter), the second question was `trial.questions[0]`, and the final question was `trial.questions[1]`. This will be encoded as a JSON string when data is saved using the `.json()` or `.csv()` functions. |

## Examples

???+ example "Basic single page"
    === "Code"

        ```javascript
        var trial = {
          type: jsPsychSurvey,
          pages: [
            [
              {
                type: 'html',
                prompt: 'Please answer the following questions:',
              },
              {
                type: 'multi-choice',
                prompt: "Which of the following do you like the most?", 
                name: 'VegetablesLike', 
                options: ['Tomato', 'Cucumber', 'Eggplant', 'Corn', 'Peas'], 
                required: true
              }, 
              {
                type: 'multi-select',
                prompt: "Which of the following do you like?", 
                name: 'FruitLike', 
                options: ['Apple', 'Banana', 'Orange', 'Grape', 'Strawberry'], 
                required: false,
              }
            ]
          ],
        };
        ```

    === "Demo"
        <div style="text-align:center;">
          <iframe src="../../demos/jspsych-survey-multi-choice-demo1.html" width="90%;" height="500px;" frameBorder="0"></iframe>
        </div>

    <a target="_blank" rel="noopener noreferrer" href="../../demos/jspsych-survey-multi-choice-demo1.html">Open demo in new tab</a>

???+ example "Multiple pages, more customization"
    === "Code"

        ```javascript
        var trial = {
          type: jsPsychSurvey,
          pages: [
            [
              {
                type: 'text',
                prompt: "Where were you born?", 
                placeholder: 'City, State, Country',
                name: 'birthplace', 
                required: true,
              }, 
              {
                type: 'text'
                prompt: "How old are you?", 
                name: 'age', 
                textbox_columns: 5,
                required: false,
              }
            ],
            [
              {
                type: 'multi-choice',
                prompt: "What&#39;s your favorite color?", 
                options: ['blue','yellow','pink','teal','orange','lime green','other','none of these'],
                name: 'FavColor', 
              }, 
              {
                type: 'multi-select'
                prompt: "Which of these animals do you like? Select all that apply.", 
                options: ['lion','squirrel','badger','whale'],
                option_reorder: 'random',
                columns: 0,
                name: 'AnimalLike', 
              }
            ]
          ],
          randomize_question_order: true,
          title: 'Demographic questions',
          button_label_next: 'Continue',
          button_label_back: 'Previous',
          button_label_finish: 'Submit'
        };
        ```

    === "Demo"
        <div style="text-align:center;">
          <iframe src="../../demos/jspsych-survey-multi-choice-demo2.html" width="90%;" height="500px;" frameBorder="0"></iframe>
        </div>

    <a target="_blank" rel="noopener noreferrer" href="../../demos/jspsych-survey-multi-choice-demo2.html">Open demo in new tab</a>