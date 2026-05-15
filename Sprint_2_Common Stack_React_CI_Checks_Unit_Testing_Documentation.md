# Common Stack | React | CI Checks | Unit Testing Documentation

---

# Author Table

| Author      | Created on | Version | Last updated by | Last Edited On | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 15-05-2026 | 1.0     | Saransh Rai     | 15-05-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---
# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Unit Testing](#2-what-is-unit-testing)
3. [Why Unit Testing is Required](#3-why-unit-testing-is-required)
4. [React Unit Testing Workflow](#4-react-unit-testing-workflow)
   - [4.1 Workflow Diagram](#41-workflow-diagram)
5. [Different Tools for React Unit Testing](#5-different-tools-for-react-unit-testing)
6. [Tool Comparison](#6-tool-comparison)
7. [Advantages and Disadvantages](#7-advantages-and-disadvantages)
8. [Proof of Concept (POC)](#8-proof-of-concept-poc)
   - [8.1 Prerequisites](#81-prerequisites)
   - [8.2 Step by Step Implementation](#82-step-by-step-implementation)
   - [8.3 POC Conclusion](#83-poc-conclusion)
9. [Best Practices](#9-best-practices)
10. [Recommendation / Conclusion](#10-recommendation--conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)
---

# 1. Introduction

React applications require proper testing to ensure that components, functions, and user interface behavior work as expected before deployment. Unit testing is one of the most important CI checks for React applications because it validates small parts of the application independently. When unit testing is integrated with CI/CD pipelines, every code change can be automatically tested before merge or deployment.

---

# 2. What is Unit Testing

Unit Testing is a software testing method where individual units of code are tested separately to confirm that they work correctly. In React, unit testing is mainly used to test components, props, hooks, rendering behavior, events, and expected UI output. The goal is to verify that each component works correctly in isolation before it becomes part of the complete application.

---

# 3. Why Unit Testing is Required

Unit testing is required because it helps identify bugs early in the development lifecycle. It improves code quality, reduces production failures, supports safe refactoring, and gives confidence to developers before merging changes. In CI/CD workflows, unit tests act as an automated quality gate that prevents faulty code from moving forward.

---

# 4. React Unit Testing Workflow

The React unit testing workflow starts when a developer writes or updates a React component. Test cases are created for the component and pushed along with the code to the Git repository. After the code is pushed, the CI pipeline is triggered automatically. The pipeline installs dependencies, runs unit tests, generates test results, and decides whether the code is safe to merge or deploy.

## <a name="41-workflow-diagram"></a>&nbsp;&nbsp;&nbsp;&nbsp; 4.1 Workflow Diagram

<details>
<summary>Click to Expand React Unit Testing Workflow Diagram</summary>

```text
Developer Writes React Code
        ↓
Developer Adds Unit Test Cases
        ↓
Code is Pushed to Git Repository
        ↓
CI Pipeline is Triggered
        ↓
Dependencies are Installed
        ↓
Unit Tests are Executed
        ↓
Test Report is Generated
        ↓
Pass → Merge / Deploy
Fail → Fix Code and Re-run Pipeline
```

</details>

---

# 5. Different Tools for React Unit Testing

| Tool                      | Description                                                                                                              |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Jest                      | JavaScript testing framework commonly used with React. It provides test runner, assertions, mocks, and coverage reports. |
| React Testing Library     | Testing library used to test React components from the user's perspective.                                               |
| Vitest                    | Fast testing framework mostly used with Vite-based React applications.                                                   |
| Cypress Component Testing | Used for component testing inside a real browser environment.                                                            |
| Enzyme                    | Older React testing utility used for shallow rendering and component testing.                                            |

---

# 6. Tool Comparison

| Tool                  | Main Purpose                     | Speed     | Ease of Use | Best Use Case                                              |
| --------------------- | -------------------------------- | --------- | ----------- | ---------------------------------------------------------- |
| Jest                  | Unit testing and assertions      | Fast      | Easy        | React applications using Create React App or react-scripts |
| React Testing Library | Component behavior testing       | Fast      | Easy        | Testing UI behavior like a real user                       |
| Vitest                | Modern unit testing              | Very Fast | Easy        | Vite-based React applications                              |
| Cypress               | Component and end-to-end testing | Moderate  | Moderate    | Browser-based testing                                      |
| Enzyme                | Component testing                | Moderate  | Moderate    | Legacy React projects                                      |

---

# 7. Advantages and Disadvantages

| Advantages                             | Disadvantages                                         |
| -------------------------------------- | ----------------------------------------------------- |
| Detects bugs early before deployment   | Requires time to write test cases                     |
| Improves code quality and reliability  | Initial setup effort is required                      |
| Supports safe refactoring              | Test maintenance is needed when UI changes            |
| Can be integrated with CI/CD pipelines | Complex UI behavior can be difficult to test          |
| Increases developer confidence         | Mocking API calls and dependencies can be challenging |

---

# 8. Proof of Concept (POC)

This POC demonstrates how unit testing can be executed in a React frontend application using Jest. The frontend repository is cloned, dependencies are installed, Jest availability is verified, a test file is created, and the unit test is executed successfully.

## <a name="81-prerequisites"></a>&nbsp;&nbsp;&nbsp;&nbsp; 8.1 Prerequisites

Before starting the POC, ensure the following tools are installed:

| Tool                       | Purpose                                              |
| -------------------------- | ---------------------------------------------------- |
| Node.js                    | Runtime environment to run JavaScript applications   |
| npm                        | Package manager used to install project dependencies |
| Git                        | Version control system used to clone the repository  |
| Frontend Repository Access | Required to clone and run the React project          |

---

## <a name="82-step-by-step-implementation"></a>&nbsp;&nbsp;&nbsp;&nbsp; 8.2 Step by Step Implementation

### Step 1: Clone the Frontend Repository

```bash
git clone https://github.com/OT-MICROSERVICES/frontend.git
cd frontend
```

<details>
<summary>Click to Expand Repository Clone Screenshot</summary>

<br>

<img width="1485" height="338" alt="Repository Clone Screenshot" src="https://github.com/user-attachments/assets/41bd39fa-521e-4472-8508-069ab684b852" />

</details>

---

### Step 2: Install Project Dependencies

```bash
npm install
```

This command installs all required project dependencies mentioned in the `package.json` file.

---

### Step 3: Verify Jest is Available

Check whether Jest is already present in the project.

```bash
npm list jest
```

<details>
<summary>Click to Expand Jest Verification Screenshot</summary>

<br>

<img width="1134" height="108" alt="Jest Verification Screenshot" src="https://github.com/user-attachments/assets/17f9bc56-7f48-4a55-b4f0-549278e5559d" />

</details>

This confirms that Jest is already included through `react-scripts`.

---

### Step 4: Create Test File

```bash
nano App.test.js
```

<details>
<summary>Click to Expand Test File Creation Screenshot</summary>

<br>

<img width="1476" height="452" alt="Test File Creation Screenshot" src="https://github.com/user-attachments/assets/0f75f7c6-ad27-4428-a53e-f7c14571f6ae" />

</details>

Sample test case:

```javascript
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders app component successfully', () => {
  render(<App />);
  const element = screen.getByText(/welcome/i);
  expect(element).toBeInTheDocument();
});
```

---

### Step 5: Run Unit Tests

Run the test command.

```bash
npm test
```

This command executes the available unit test cases using Jest.

---

### Step 6: Output

After running the test, the terminal should show successful test execution.

<details>
<summary>Click to Expand Unit Test Output Screenshot</summary>

<br>

<img width="1622" height="463" alt="Unit Test Output Screenshot" src="https://github.com/user-attachments/assets/85ab7f32-5b05-40b5-be76-624e77ee289f" />

</details>

This confirms that the unit test executed successfully using Jest.

---

## <a name="83-poc-conclusion"></a>&nbsp;&nbsp;&nbsp;&nbsp; 8.3 POC Conclusion

This POC demonstrates how unit testing can be executed in a React frontend application using Jest. A test case was created and executed to verify that the application component renders successfully. This proves that React unit testing can be added as a CI check to validate frontend code before deployment.

---

# 9. Best Practices

| Best Practice                 | Description                                                       |
| ----------------------------- | ----------------------------------------------------------------- |
| Write small and focused tests | Each test should validate one specific behavior                   |
| Use meaningful test names     | Test names should clearly explain what is being verified          |
| Test user behavior            | Focus on what the user sees and does, not internal implementation |
| Avoid unnecessary snapshots   | Use snapshots only when they add value                            |
| Run tests in CI pipeline      | Ensure every code change is validated automatically               |
| Maintain test coverage        | Important components and functions should be covered              |
| Mock external dependencies    | Avoid dependency on real APIs during unit testing                 |

---

# 10. Recommendation / Conclusion

For React unit testing, **Jest with React Testing Library** is the recommended approach. Jest is already available in many React projects through `react-scripts`, and React Testing Library helps test components from the user's perspective. This combination is simple, widely used, CI/CD friendly, and suitable for validating React components before deployment.

---

# 11. Contact Information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Saransh Rai | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

# 12. References

| Topic                                                                                                | Description                                                  |
| ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| [React Testing Library Documentation](https://testing-library.com/docs/react-testing-library/intro/) | Official React Testing Library documentation                 |
| [Jest Documentation](https://jestjs.io/docs/getting-started)                                         | Official Jest documentation                                  |
| [Vitest Documentation](https://vitest.dev/guide/)                                                    | Official Vitest documentation                                |
| [Cypress Documentation](https://docs.cypress.io/)                                                    | Official Cypress documentation                               |
| [Create React App Testing](https://create-react-app.dev/docs/running-tests/)                         | React testing documentation for react-scripts based projects |
