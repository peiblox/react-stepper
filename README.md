# React material stepper

Implementation of [Material Stepper] for React. **React material stepper** encapsulates logic of step state maintianing, and provides API for control over the step resolution

[Material Stepper]: https://material.io/archive/guidelines/components/steppers.html

## Features

- Simple, declarative configuration
- Typescript typings
- Horizontal and vertical layouts
- Dynamic steps
- locking
- Default material themes provided
- Customizable by SCSS

## Examples

- [Simple horizontal stepper][simple] ([source][simple_src])
- [Simple verical stepper][vertical] ([source][vertical_src])
- [Full featured stepper][advanced] ([source][advanced_src])

[simple]: https://apuchenkin.github.io/react-stepper/example/dist/simple
[vertical]: https://apuchenkin.github.io/react-stepper/example/dist/vertical
[advanced]: https://apuchenkin.github.io/react-stepper/example/dist/full

[simple_src]: example/src/simple
[vertical_src]: example/src/vertical
[advanced_src]: example/src/advanced

## Getting started

```jsx
import Stepper, { Step } from "react-material-stepper";

const StepperExample = () => (
  <Stepper>
    <Step
      stepId={STEP1}
      data="Step 1 initial state"
      title="Step One"
      description="This step is optional"
    >
      <Step1 />
    </Step>
    <Step stepId={STEP2} title="Step Two" description="Login is required">
      <Step2 />
    </Step>
    <Step stepId={STEP3} title="Step Three" >
      <Step3 />
    </Step>
  </Stepper>
);

```
*Example1: Basic stepper configuration, where `Step1`, `Step2` and `Step3` is arbitary user defined components*

In order to work with stepper API `StepperContext` could be used:

```jsx
import {
  StepperAction,
  StepperContent,
  StepperContext
} from "react-material-stepper";

export const STEP1 = "step-one";

const Step1 = ({ vertical = false }) => {
  const { resolve, getData } = React.useContext(StepperContext);

  const data = getData(STEP1);

  const onSubmit = (event: React.FormEvent) => {
    event.preventDefault();
    // resolve will set data for current step and proceed to the next step
    resolve("step1 resolved data");
  };

  return (
    <StepperContent
      onSubmit={onSubmit}
      actions={
        <StepperAction align="right" type="submit">
          Continue
        </StepperAction>
      }
    >
      Step1 resolved:
      <pre>{data}</pre>
    </StepperContent>
  );
};
```
*Example2: `StepperContext` allows step data resolution. Saved data is accessible by `getData` method*


## API

### `Stepper`

| Prop        | Type                                | Description                                                          |
|-------------|-------------------------------------|----------------------------------------------------------------------|
| initialStep | string \| number                    | Stets initital step by `StepId`                                      |
| onResolve   | (stepId) => {}                      | Callback that will be executed on each step resolution               |
| onReject    | (stepId) => {}                      | Callback that will be executed on each step rejection                |
| contextRef  | MutableRefObject<StepperController> | Stepper controller reference                                         |
| className   | string                              | Custom classname will be added to the root stepper container         |
| vertical    | boolean                             | Indicates either horizontal or vertical steppr layout should be used |

### `Step`

```jsx
<Step
  stepId="2"
  title="Step Two"
  loading={isLoading(STEP2)}
  description="Login is required"
>
  ...
</Step>
```
*Example3: `Step` component describes step configuration*

| Prop        | Type            | Description                                      |
|-------------|-----------------|--------------------------------------------------|
| **title***  | string          | Step title. Required                             |
| **stepId*** | string \| number | Unique step identifier. Required                |
| **children*** | ReactNode     | React component that will be rendered when step is activated. Required |
| description | string          | Step description or hint. Optional               |
| loading     | boolean         | Indicates whether step content is beign loading. |
| disabled    | boolean         | Indicates whether step is beign disabled         |
| data        | any             | Initial state of step                            |
| className   | string          | Custom classname                                 |
| index       | number          | Step index                                       |

### `StepperContext`

Provides API for control over stepper

| Prop           | Type                         | Description                                                       |
|----------------|------------------------------|-------------------------------------------------------------------|
| updateStep     | (stepId, state) => {}        | Updates step state by id.                                         |
| goAt           | stepId => {}                 | Activates certain step at provided stepId                         |
| resolve        | data => {}                   | Resolves current step with provided data                          |
| reject         | (message, description) => {} | Rejects current step with error and description                   |
| isLoading      | () => boolean                | Indicates whether any of stepper steps is being loading           |
| getCurrentStep | () => StepState              | Returns current step state                                        |
| getStep        | stepId => StepState          | Returns step state by stepId                                      |
| getData        | (stepId, fallback)           | Returns step data, fallback is used when step data is not defined |

### `StepperContent`

`StepperContent` extends `form` interface, Could be used in custom steps for convenience and styling.
Additionally `StepperContent` accept `actions` prop, that will be rendered in the footer of stepper content

### `StepperAction`

`StepperAction` extends `button` interface, Could be used in custom steps for convenience and styling.
Additionally `StepperAction` accept `align` ('left' or 'right') prop.

## Customization

### As part of [material theme]

```scss
$mdc-theme-primary: #fcb8ab;
$mdc-theme-secondary: #feeae6;
$mdc-theme-on-primary: #442b2d;
$mdc-theme-on-secondary: #442b2d;

@import "react-material-stepper/src/index.scss";
@import "material-components-web/material-components-web";
```

[Material theme]: https://github.com/material-components/material-components-web/tree/master/packages/mdc-theme

### By SCSS variables

```scss
$stepper-color-hover: lightblue;
$stepper-color-index: darkblue;
$stepper-color-success: green;
$stepper-color-error: red;
$stepper-color-connector: cornflowerblue;
$stepper-header-height-horizontal: 64px;

@import "react-material-stepper/src/index.scss";
```

### By CSS override

Stepper components allows passing custom `className`, and use it for override existing styles by applying css rules

```jsx
import 'react-material-stepper/dist/react-stepper.css';
```

```jsx
<Stepper className="custom-theme">
  <Step
    stepId={STEP1}
    title="Step One"
  >
    <Step1 />
  </Step>
  ...
</Stepper>
```


```css
.stepper.custom-theme {
  background: red;
}
```