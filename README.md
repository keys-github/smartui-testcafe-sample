# SmartUI SDK Sample for TestCafe — TestMu AI (Formerly LambdaTest)

Welcome to the SmartUI SDK sample for TestCafe. This repository demonstrates how to integrate SmartUI visual regression testing with TestCafe.

## Repository Structure

```
smartui-testcafe-sample/
├── testcafeSDKLocal.js    # Test file (works for both Local and Cloud)
├── package.json            # Dependencies
└── smartui-web.json        # SmartUI config (create with npx smartui config:create)
```

## 1. Prerequisites and Environment Setup

### Prerequisites

- Node.js installed
- TestMu AI account credentials (for Cloud tests)
- Chrome browser (for Local tests)

### Environment Setup

**For Cloud:**
```bash
export LT_USERNAME='your_username'
export LT_ACCESS_KEY='your_access_key'
export PROJECT_TOKEN='your_project_token'
```

**For Local:**
```bash
export PROJECT_TOKEN='your_project_token'
```

## 2. Initial Setup and Dependencies

### Clone the Repository

```bash
git clone https://github.com/LambdaTest/smartui-testcafe-sample
cd smartui-testcafe-sample
```

### Install Dependencies

The repository already includes the required dependencies in `package.json`. Install them:

```bash
npm install
```

**Dependencies included:**
- `@lambdatest/smartui-cli` - SmartUI CLI
- `@lambdatest/testcafe-driver` - SmartUI TestCafe driver
- `testcafe` - TestCafe framework

**For Cloud execution, also install:**
```bash
npm install testcafe-browser-provider-lambdatest
```

### Create SmartUI Configuration

```bash
npx smartui config:create smartui-web.json
```

## 3. Steps to Integrate Screenshot Commands into Codebase

The SmartUI screenshot function is already implemented in the repository.

**Test File** (`testcafeSDKLocal.js`):
```javascript
import { smartuiSnapshot } from '@lambdatest/testcafe-driver';

fixture('LambdaTest Test')
  .page('https://www.lambdatest.com');

test('Take Homepage Screenshot', async (t) => {
  await smartuiSnapshot(t, 'screenshot');
});
```

**Note**: The code is already configured and ready to use. You can modify the URL and screenshot name if needed. The `smartuiSnapshot` function takes the test controller `t` as the first parameter and the screenshot name as the second parameter.

## 4. Execution and Commands

### Local Execution

```bash
npx smartui exec -- npx testcafe chrome testcafeSDKLocal.js
```

### Cloud Execution

```bash
npx smartui exec -- npx testcafe "lambdatest:Chrome@latest:Windows 10" testcafeSDKLocal.js
```

**Note**: Replace `"lambdatest:Chrome@latest:Windows 10"` with your desired browser and platform.

## Test File

The test file (`testcafeSDKLocal.js`) works for both local and cloud execution.

## Configuration

### SmartUI Config (`smartui-web.json`)

Create the SmartUI configuration file using:
```bash
npx smartui config:create smartui-web.json
```

## Best Practices

### Screenshot Naming

- Use descriptive, unique names for each screenshot
- Include test context and state
- Avoid special characters
- Use consistent naming conventions

### When to Take Screenshots

- After critical user interactions
- Before and after form submissions
- At different viewport sizes
- After page state changes

### TestCafe-Specific Tips

- Use `await t.wait()` before screenshots for dynamic content
- Take screenshots after page loads completely
- Use `t.resizeWindow()` for responsive testing
- Combine with TestCafe assertions for better test flow

### Example: Screenshot After Interaction

```javascript
import { smartuiSnapshot } from '@lambdatest/testcafe-driver';

fixture('LambdaTest Test')
  .page('https://www.lambdatest.com');

test('Take Screenshot After Search', async (t) => {
  await t.typeText('#search', 'TestCafe');
  await t.wait(1000);
  await smartuiSnapshot(t, 'search-results');
});
```

## Common Use Cases

### Responsive Testing

```javascript
fixture('Responsive Tests')
  .page('https://www.lambdatest.com');

test('Desktop View', async (t) => {
  await t.resizeWindow(1920, 1080);
  await smartuiSnapshot(t, 'homepage-desktop');
});

test('Tablet View', async (t) => {
  await t.resizeWindow(768, 1024);
  await smartuiSnapshot(t, 'homepage-tablet');
});

test('Mobile View', async (t) => {
  await t.resizeWindow(375, 667);
  await smartuiSnapshot(t, 'homepage-mobile');
});
```

### Multi-Step Flow Testing

```javascript
test('Checkout Flow', async (t) => {
  await t.navigateTo('https://example.com/checkout');
  await smartuiSnapshot(t, 'checkout-step-1');
  
  await t.click('#next-step');
  await t.wait(500);
  await smartuiSnapshot(t, 'checkout-step-2');
});
```

## CI/CD Integration

### GitHub Actions Example

```yaml
name: TestCafe SmartUI Tests

on: [push, pull_request]

jobs:
  visual-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run TestCafe with SmartUI (Local)
        env:
          PROJECT_TOKEN: ${{ secrets.SMARTUI_PROJECT_TOKEN }}
        run: |
          npx smartui exec -- npx testcafe chrome testcafeSDKLocal.js
      
      - name: Run TestCafe with SmartUI (Cloud)
        env:
          PROJECT_TOKEN: ${{ secrets.SMARTUI_PROJECT_TOKEN }}
          LT_USERNAME: ${{ secrets.LT_USERNAME }}
          LT_ACCESS_KEY: ${{ secrets.LT_ACCESS_KEY }}
        run: |
          npx smartui exec -- npx testcafe "lambdatest:Chrome@latest:Windows 10" testcafeSDKLocal.js
```

## Troubleshooting

### Issue: `smartuiSnapshot is not a function`

**Solution**: Ensure the driver is imported:
```javascript
import { smartuiSnapshot } from '@lambdatest/testcafe-driver';
```

### Issue: Screenshots not captured

**Solution**:
1. Verify `PROJECT_TOKEN` is set
2. Add waits before screenshots
3. Ensure test completes successfully
4. Check TestCafe version compatibility

### Issue: `PROJECT_TOKEN is required`

**Solution**: Set the environment variable:
```bash
export PROJECT_TOKEN='your_project_token'
```

### Issue: Cloud execution fails

**Solution**:
1. Install browser provider: `npm install testcafe-browser-provider-lambdatest`
2. Verify `LT_USERNAME` and `LT_ACCESS_KEY` are set
3. Check browser/platform format: `"lambdatest:Chrome@latest:Windows 10"`

## Configuration Tips

### Optimizing `smartui-web.json`

```json
{
  "web": {
    "browsers": ["chrome", "firefox", "edge"],
    "viewports": [
      [1920, 1080],
      [1366, 768],
      [375, 667]
    ],
    "waitForPageRender": 30000,
    "waitForTimeout": 2000
  }
}
```

## View Results

After running the tests, visit your SmartUI project dashboard to view the captured screenshots and compare them with baseline builds.

## Additional Resources

- [SmartUI TestCafe Onboarding Guide](https://www.testmuai.com/support/docs/smartui-onboarding-testcafe/)
- [TestCafe Documentation](https://testcafe.io/documentation/)
- [TestMu AI TestCafe Documentation](https://www.testmuai.com/support/docs/testcafe-testing/)
- [SmartUI Dashboard](https://smartui.lambdatest.com/)
- [TestMu AI Community](https://community.testmuai.com/)

## 🚀 [LambdaTest is Now TestMu AI](https://www.testmuai.com/lambdatest-is-now-testmuai/)

👋 Welcome to TestMu AI, the next evolution of LambdaTest. As of January 2026, LambdaTest has officially rebranded to TestMu AI. We have evolved from a cross-browser testing cloud into a unified, AI-native quality engineering platform designed for the modern DevOps era.

Whether you have been part of the LambdaTest community for years or are just discovering TestMu AI, our mission remains the same: to help you ship faster with high-scale test execution, autonomous testing, and deep quality analytics.

**🔄 Our Rebrand Journey**

We chose the name TestMu AI to reflect our shift towards intelligent, autonomous testing. While our identity has changed, our core technology and commitment to the testing community stay the same.

**✨ Specialties**

- 🤖 AI-Native Test Execution (Formerly LambdaTest)
- ⚡ Autonomous Test Automation
- 🌐 Cross-Browser & Mobile Testing
- 📊 Unified Quality Intelligence

👉 Find [LambdaTest's New Home](https://www.testmuai.com/).