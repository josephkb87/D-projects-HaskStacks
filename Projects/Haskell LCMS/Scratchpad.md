
# Learning Content Management System made with Haskell

## 1. Set Up Development Environment

### Install Necessary Tools

- **Haskell**: Install GHC and Stack.
  ```sh
  curl -sSL https://get.haskellstack.org/ | sh
  ```

- **Firebase CLI**: Install Firebase tools.
  ```sh
  npm install -g firebase-tools
  ```

- **Moodle**: Install and configure Moodle.
  - Follow the [Moodle installation guide](https://docs.moodle.org/400/en/Installation_quick_guide).

## 2. Initialize Haskell Project

- Create a new Haskell project using Stack.
  ```sh
  stack new lcms
  cd lcms
  ```

## 3. Configure Firebase

- Create a Firebase project in the Firebase Console.
- Enable authentication methods (email/password, Google, etc.).
- Download the `firebaseConfig` JSON file from Firebase Console.

## 4. Set Up Backend with Haskell

### Choose a Web Framework

- **Yesod**: A powerful Haskell web framework.
  ```sh
  stack new my-yesod-app yesod-sqlite
  cd my-yesod-app
  stack build
  ```

### Install Dependencies

Add dependencies to `package.yaml` or `stack.yaml`:
```yaml
dependencies:
  - yesod
  - yesod-form
  - yesod-auth
  - firebase-auth
  - http-conduit
  - aeson
  - text
```

### Define API Endpoints

Create handlers for registration, login, and content management:
```haskell
{-# LANGUAGE OverloadedStrings #-}
{-# LANGUAGE QuasiQuotes #-}
{-# LANGUAGE TemplateHaskell #-}
{-# LANGUAGE TypeFamilies #-}
{-# LANGUAGE ViewPatterns #-}

module Handler.Home where

import Import
import Firebase
import Network.HTTP.Conduit (simpleHttp)
import Data.Aeson (eitherDecode)

postRegisterR :: Handler Value
postRegisterR = do
    email <- runInputPost $ ireq textField "email"
    password <- runInputPost $ ireq textField "password"
    result <- liftIO $ registerUser email password
    return $ object ["result" .= result]

postLoginR :: Handler Value
postLoginR = do
    email <- runInputPost $ ireq textField "email"
    password <- runInputPost $ ireq textField "password"
    result <- liftIO $ loginUser email password
    return $ object ["result" .= result]

registerUser :: Text -> Text -> IO (Either String String)
registerUser email password = do
    -- Firebase registration API call
    ...

loginUser :: Text -> Text -> IO (Either String String)
loginUser email password = do
    -- Firebase login API call
    ...
```

## 5. Integrate with Moodle

### Enable Moodle Web Services

- Enable web services in Moodle.
- Create a new external service and add functions.
- Generate a token for API access.

### Interface with Moodle from Haskell

- Use HTTP client libraries to call Moodle API endpoints.
```haskell
getMoodleCourses :: IO (Either String [Course])
getMoodleCourses = do
    response <- simpleHttp "http://your-moodle-site/webservice/rest/server.php?..."
    return $ eitherDecode response
```

## 6. Frontend Development

### Choose Frontend Framework

- **React**: Popular for modern web apps.
  ```sh
  npx create-react-app lcms-frontend
  cd lcms-frontend
  npm install firebase
  ```

### Integrate Firebase Authentication

- Initialize Firebase in your React app:
```javascript
import firebase from "firebase/app";
import "firebase/auth";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};

if (!firebase.apps.length) {
  firebase.initializeApp(firebaseConfig);
}
```

### Connect Frontend to Backend

- Use Axios or Fetch to connect the frontend to the Haskell backend.
```javascript
import axios from 'axios';

const register = (email, password) => {
  return axios.post('/register', { email, password });
};

const login = (email, password) => {
  return axios.post('/login', { email, password });
};
```

## 7. Unit Testing with Jest

### Set Up Jest

- Install Jest and React Testing Library:
  ```sh
  npm install --save-dev jest @testing-library/react @testing-library/jest-dom
  ```

### Configure Jest

- Create `jest.config.js`:
```javascript
module.exports = {
  setupFilesAfterEnv: ["@testing-library/jest-dom/extend-expect"],
  testEnvironment: "jsdom",
};
```

### Write Tests

- Create a sample test file `App.test.js`:
```javascript
import React from 'react';
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders login button', () => {
  render(<App />);
  const loginButton = screen.getByText(/login/i);
  expect(loginButton).toBeInTheDocument();
});
```

## 8. CI/CD Pipeline with Tekton

### Install Tekton

- Follow the Tekton installation instructions for your Kubernetes cluster.
  - [Tekton Installation](https://tekton.dev/docs/getting-started/#installation)

### Create Tekton Resources

- **Pipeline Resource** (`pipeline.yaml`):
```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: lcms-pipeline
spec:
  tasks:
  - name: build
    taskRef:
      name: build-task
  - name: test
    taskRef:
      name: test-task
    runAfter:
      - build
  - name: deploy
    taskRef:
      name: deploy-task
    runAfter:
      - test
```

- **Build Task** (`build-task.yaml`):
```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: build-task
spec:
  steps:
  - name: build
    image: node:14
    script: |
      npm install
      npm run build
```

- **Test Task** (`test-task.yaml`):
```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: test-task
spec:
  steps:
  - name: test
    image: node:14
    script: |
      npm install
      npm test
```

- **Deploy Task** (`deploy-task.yaml`):
```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: deploy-task
spec:
  steps:
  - name: deploy
    image: google/cloud-sdk:slim
    script: |
      gcloud app deploy
```

- **PipelineRun** (`pipeline-run.yaml`):
```yaml
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  name: lcms-pipeline-run
spec:
  pipelineRef:
    name: lcms-pipeline
```

## 9. Deploy and Test

- Deploy the backend, frontend, and Moodle instance.
- Run tests and verify the integration.
- Monitor and iterate based on feedback.


===


# LCMS 

# HaskellStacks Learning Content Management System with Haskell, Moodle, and Firebase

It is apart of the HaskStacks for the Haskell Stack, the Functional Programming language. 


For [Worked Examples](#) and, [Haskell Projects](#) made with Haskell. 

## [Haskell Projects](#)

* [Accmeter](../Projects/insslamgnss)

* [Magnetometer](../Projects/Magmeter)

## [Docs](../docs/docs.md)  [CHANGELOG]((../docs/CHANGELOG.md))  [Contribute](../docs/CONTRIBUTING.md)  [Pull Requests](../docs/blob/PRs.md)


