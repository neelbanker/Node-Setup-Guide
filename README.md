## Node-Setup-Guide
sudo npm install -g express-generator

express --view=hbs folder_name

git init

git add --all

git commit -m "Initial Commit"

git remote add origin REPO_URL

git push -u origin master

npm install


OPEN :: https://github.com/goldbergyoni/nodebestpractices

***************************************************************************************
## Instalation of ESLint

npm install eslint --save-dev

./node_modules/.bin/eslint --init

npm install --save-dev eslint eslint-plugin-node

npm install --save-dev eslint-plugin-security

========================================= ES Config file =====================================================

module.exports = {
  env: {
    es6: true,
    node: true,
  },
  extends: [
    'airbnb-base',
    'plugin:node/recommended',
    'plugin:security/recommended',
  ],
  plugins: ['security'],
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly',
  },
  parserOptions: {
    ecmaVersion: 2018,
    sourceType: 'module',
  },
  rules: {
    'security/detect-eval-with-expression': 'error',
    'node/exports-style': ['error', 'module.exports'],
    'node/no-exports-assign': 'error',
    'node/no-extraneous-import': 'error',
    'node/no-unsupported-features/es-builtins': 'error',
    'node/no-unsupported-features/es-syntax': 'off',
    'node/no-unsupported-features/node-builtins': 'error',
    'node/no-missing-import': 'error',
    'node/no-extraneous-require': 'error',
    'node/no-missing-require': 'error',
    'node/no-unpublished-import': 'error',
    'node/no-unpublished-require': 'error',
    'node/no-unpublished-bin': 'error',
    'node/process-exit-as-throw': 'error',
    'node/shebang': 'error',
    'node/no-deprecated-api': 'error',
    'node/file-extension-in-import': ['error', 'never'],
    'node/prefer-global/buffer': ['error', 'always'],
    'node/prefer-global/text-decoder': ['error', 'always'],
    'node/prefer-global/text-encoder': ['error', 'always'],
    'node/prefer-global/console': ['error', 'always'],
    'node/prefer-global/process': ['error', 'always'],
    'node/prefer-global/url-search-params': ['error', 'always'],
    'node/prefer-global/url': ['error', 'always'],
    'node/prefer-promises/dns': 'error',
    'node/prefer-promises/fs': 'error',
    'security/detect-buffer-noassert': 'error',
    'security/detect-child-process': 'error',
    'security/detect-disable-mustache-escape': 'error',
    'security/detect-eval-with-expression': 'error',
    'security/detect-new-buffer': 'error',
    'security/detect-no-csrf-before-method-override': 'error',
    'security/detect-non-literal-fs-filename': 'error',
    'security/detect-non-literal-regexp': 'error',
    'security/detect-non-literal-require': 'error',
    'security/detect-object-injection': 'error',
    'security/detect-possible-timing-attacks': 'error',
    'security/detect-pseudoRandomBytes': 'error',
    'security/detect-unsafe-regex': 'error',
    'no-useless-escape': 0,
    'no-underscore-dangle': 0,
    'no-throw-literal': 0,
    'global-require': 0,
    'no-param-reassign': 0,
    'no-plusplus': 0,
    'max-len':0,
    'no-useless-catch':0,
  },
};


========================================================================

***************************************************************************************

npm i prettier

npm install -D eslint-config-prettier eslint-plugin-prettier
 
install prettier in vscode

-> create file .prettierrc

place this code in it :

{
    "tabWidth": 2,
    "semi": true,
    "singleQuote": true,
    "trailingComma": "all"
}

-> create folder .vscode and create file in it named settings.json

place this code in it :


{
    "[javascript]": {
        "editor.formatOnSave": true
    },
}


***************************************************************************************

npm i winston --save

create logger.js inside common/middlewares


place this code in it :

=================================================================

const winston = require('winston');

winston.emitErrs = true;

const logger = new winston.Logger({
  transports: [
    new winston.transports.File({
      level: 'info',
      filename: './logs/all-logs.log',
      handleExceptions: true,
      json: true,
      maxsize: 5242880, // 5MB
      maxFiles: 5, // if log file size is greater than 5MB, logfile2 is generated
      colorize: true,
    }),
    new winston.transports.Console({
      level: 'debug',
      handleExceptions: true,
      json: false,
      colorize: true,
      timestamp: true,
    }),
  ],
  exceptionHandlers: [
    new winston.transports.File(
      {
        filename: './logs/exceptions.log',
        timestamp: true,
        maxsize: 5242880,
        json: true,
        colorize: true,
      },
    ),
  ],
  exitOnError: false,
});


module.exports = logger;
module.exports.stream = {
  write(message) {
    logger.info(message);
  },
};

========================================================================

***************************************************************************************

create file named .env

add code in it :


# Mongo DB
# Local development
MONGODB_URI='mongodb://localhost/dream_start'

# Port
PORT=3000

# Debug 
LOG_LEVEL='debug'

#NODE_ENV='development'

***************************************************************************************


npm install --save express-rate-limit

npm install --save rate-limit-mongo


***************************************************************************************

store all configs like secret keys in .env file

and access them using ::

const apiKey = process.env.AZURE_STORAGE_KEY;

***************************************************************************************

Adjust the HTTP response headers for enhanced security

npm install helmet --save


***************************************************************************************
https://github.com/goldbergyoni/nodebestpractices/blob/master/sections/security/login-rate-limit.md

https://medium.com/@animirr/brute-force-protection-node-js-examples-cd58e8bd9b8d


***************************************************************************************

npm i body-parser --save

in app.js ::

app.use(bodyParser.json({ limit: '50mb' }));
app.use(bodyParser.urlencoded({ extended: true, limit: '50mb' }));


***************************************************************************************

npm i express-session --save

in app.js ::

app.use(
  session({
    secret: 'ssshhhhh',
    resave: false,
    saveUninitialized: true,
    cookie: {
      path: '/',
      httpOnly: true,
      secure: true,
      maxAge: 60000 * 60 * 24,
    },
  }),
);

