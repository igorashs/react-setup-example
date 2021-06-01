# React | Typescript | ESLint | Prettier App Setup

### Project setup

- Start a new Create React App project with TypeScript

    ```bash
    npx create-react-app my-app --template typescript
    ```

- Add TypeScript to an existing Create React App project

    ```bash
    npm install --save typescript @types/node @types/react @types/react-dom @types/jest
    ```

### Configuration

- Absolute paths

    ```json
    // tsconfig.json
    {
      "compilerOptions": {
    		"baseUrl": "src"
    	}
    }
    ```

### Setup ESLint + Prettier

- ESLint
    - Step 1: Removing the pre-set ESLint configuration from React project

        ```json
        // package.json
        "eslintConfig": {
           "extends":[
              "react-app",
              "react-app/jest"
           ]
        }
        ```

    - Step 2: Install ESLint package

        ```bash
        npm install eslint --save-dev
        ```

    - Step 3: Setup ESLint

        ```bash
        npx eslint --init
        ```

        - Configuration questions
            - How would you like to use ESLint?
                - Select: To check syntax, find problems, and enforce code style
            - What type of modules does your project use?
                - Select: JavaScript modules (import/export)
            - Which framework does your project use?
                - Select: React
            - Does your project use TypeScript?
                - Select: Yes
            - Where does your code run?
                - Select: Browser
            - How would you like to define a style for your project?
                - Select: Use a popular style guide
            - Which style guide do you want to follow?
                - Select: Airbnb: [https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)
            - What format do you want your config file to be in?
                - Select: JSON
            - Would you like to install them now with npm?
                - Select: Yes
    - Solving problems
        - Problem: “‘no-use-before-define”
            - ex: ‘React’ was used before it was defined
            - On ‘*eslintrc.json’*, over *“rules”*, add the follow:

                ```json
                "rules": {
                	"no-use-before-define": "off",
                	"@typescript-eslint/no-use-before-define": ["error"]
                }
                ```

        - Problem: “‘react/jsx-filename-extension”
            - ex: JSX not allowed in files with extension ‘.tsx’
            - On ‘eslintrc.json’, over “rules”, add the follow:

                ```json
                "rules":{
                …
                	"react/jsx-filename-extension": [ "warn", {"extensions": [".tsx"]} ]
                }
                ```

        - Problem: “import/no-unresolved”
            - ex: Unable to resolve path to module ‘./App’
            - Inside the project directory, open a terminal and install eslint-import-resolver-typescript package

                ```bash
                npm install eslint-import-resolver-typescript --save-dev
                ```

            - On ‘eslintrc.json’, Add a new property “settings” to the json, as follow:

                ```json
                "settings": {
                	"import/resolver": {
                		"typescript": {}
                	}
                }
                ```

        - Problem: “import/extensions”
            - ex: Missing file extension ‘tsx’ for ‘./App’
            - On ‘eslintrc.json’, over “rules”, add the follow:

                ```json
                "rules":{
                …
                	"import/extensions": [
                		"error",
                		"ignorePackages",
                		{
                			"ts": "never",
                			"tsx": "never"
                		}
                	]
                }
                ```

        - Problem: “no-undef”
            - ex: ‘test’ is not defined
            - On ‘eslintrc.json’, over “extends”, add “plugin:@typescript-eslint/recommended”:

                ```json
                "extends": [
                …
                	"plugin:@typescript-eslint/recommended"
                ],
                ```

        - Problem: "react/react-in-jsx-scope"
            - ex: 'React' must be in scope when using JSX
            - On ‘eslintrc.json’, over “rules”, add the follow:

                ```json
                "rules":{
                …
                	"react/react-in-jsx-scope": "off",
                }
                ```

        - Problem: “no-shadow”
            - ‘Enum’ is already declared in the upper scope
            - On ‘eslintrc.json’, over “rules”, add the follow:

                ```json
                "rules":{
                …
                	"no-shadow": "off",
                	"@typescript-eslint/no-shadow": ["error"]
                }
                ```

        - Problem: Any error over files that are not ‘js’,’jsx’, ‘ts’, or ‘tsx’ extension files
            - You can avoid ESLint to look over some files by adding it on the ‘.eslintignore’ file.
            - Create a ‘.eslintignore’ file in the root of your project
            - Add the follow text to it:

                ```
                *.css
                *.svg
                ```

    - Some rules to ignore*
        - Prefer use of default export
            - On ‘eslintrc.json’, over “rules”, add:

                ```json
                "rules":{
                …
                	"import/prefer-default-export": "off"
                }
                ```

        - Prop Types rules
            - On ‘eslintrc.json’, over “rules”, add:

                ```json
                "rules":{
                …
                	"react/prop-types": "off"
                }
                ```

- Prettier
    - Step 1: Install Prettier package

        ```bash
        npm install --save-dev --save-exact prettier
        ```

    - Step 2: ‘.prettierrc’ file
        - Create a ‘.prettierrc’ file and add some prettier rules:

            ```json
            {
              "singleQuote": true,
              "trailingComma": "all",
              "arrowParens": "always",
              "tabWidth": 2,
              "printWidth": 80
            }
            ```

    - Step 3: ‘.prettierignore’ file
        - Create a ‘.prettierignore ‘ file and add the follow:

            ```
            node_modules
            # Ignore artifacts:
            build
            ```

- Making ESLint and Prettier work together
    - Step 1: Setup ESLint and Prettier first
    - Step 2: Disabling ESLint formatting rules
        - Install the package eslint-config-prettier

            ```bash
            npm install --save-dev eslint-config-prettier
            ```

        - On the ‘.eslintrc.json’ file, over “extends”, add the follow:

            ```bash
            "extends":[
            ...
            	"plugin:react/recommended",
            	"airbnb",
            	"prettier/prettier",
            ]
            ```

    - Step 3: Make ESLint use Prettier rules
        - Install package eslint-plugin-prettier

            ```bash
            npm install --save-dev eslint-plugin-prettier
            ```

        - On the ‘.eslintrc.json’ file, add the follow:

            ```json
            "extends":[
            ...
            	"plugin:prettier/recommended" // add as the last extension
            ]
            ```

        - *Note 1: This turns on the rule provided by* eslint-plugin-prettier *plugin, which runs Prettier from within ESLint.*
    - Step 4: VS Code: execute ESLint + Prettier with auto fix in a file when save
        - Create a ‘.vscode’ folder on the root of the project
        - Create a ‘settings.json’ file inside .vscode/ folder and insert the follow code on it:

            ```json
            {
            	"editor.defaultFormatter": "dbaeumer.vscode-eslint",
            	"editor.formatOnSave": true,
            	"eslint.alwaysShowStatus": true,
            	"editor.codeActionsOnSave": {
            		"source.fixAll.eslint": true
            	}
            }
            ```

        - Install VS Code ESLint extension

            ![ASSETS/Untitled%201.png](ASSETS/Untitled%201.png)

        - Allow ESLint extension usage on VS Code:
            1. Click on the status bar icon.

                ![ASSETS/Untitled%202.png](ASSETS/Untitled%202.png)

            2. Select Allow option

                ![ASSETS/Untitled%203.png](ASSETS/Untitled%203.png)

        - Install VS CODE Prettier extension

            ![ASSETS/Untitled%204.png](ASSETS/Untitled%204.png)

        - Using Prettier extension for JSON formatting (settings.json)

            ```json
            "[json]": {
              "editor.defaultFormatter": "esbenp.prettier-vscode",
              "editor.formatOnSave": true
            },
            "[jsonc]": {
            	"editor.defaultFormatter": "esbenp.prettier-vscode",
              "editor.formatOnSave": true
            }
            ```

- Example .eslintrc.json

    U can't just copy it 👹

    ```json
    {
      "env": {
        "browser": true,
        "es2021": true
      },
      "settings": {
        "import/resolver": {
          "typescript": {}
        }
      },
      "extends": [
        "plugin:react/recommended",
        "airbnb",
        "prettier/prettier",
        "plugin:@typescript-eslint/recommended",
        "plugin:prettier/recommended"
      ],
      "parser": "@typescript-eslint/parser",
      "parserOptions": {
        "ecmaFeatures": {
          "jsx": true
        },
        "ecmaVersion": 12,
        "sourceType": "module"
      },
      "plugins": ["react", "@typescript-eslint"],
      "rules": {
        "no-use-before-define": "off",
        "@typescript-eslint/no-use-before-define": ["error"],
        "react/jsx-filename-extension": ["warn", { "extensions": [".tsx"] }],
        "import/extensions": [
          "error",
          "ignorePackages",
          {
            "ts": "never",
            "tsx": "never"
          }
        ],
        "react/react-in-jsx-scope": "off",
        "no-shadow": "off",
        "@typescript-eslint/no-shadow": ["error"],
        "import/prefer-default-export": "off",
        "react/prop-types": "off"
      }
    }
    ```

- Sources

    [Setting ESLint on a React Typescript project 2021](https://andrebnassis.medium.com/setting-eslint-on-a-react-typescript-project-2021-1190a43ffba)

    [Setting Prettier on a React Typescript project (2021)](https://andrebnassis.medium.com/setting-prettier-on-a-react-typescript-project-2021-f9f0d5a1d6b0)

    [How to add ESLint and Prettier to a React TypeScript Project (2021)](https://javascript.plainenglish.io/setting-eslint-and-prettier-on-a-react-typescript-project-2021-22993565edf9)

---