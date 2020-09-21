# Testing
## Enzyme basic example

```js
import React from 'react';
import { act } from 'react-dom/test-utils';
import { mount, render, shallow } from 'enzyme';
import CompanyLogoEdit from './';

describe('MyComponent', () => {
  it('renders without crashing', () => {
    expect(shallow.bind(shallow, <MyComponent />))
      .not
      .toThrow();
  });

  it('renders as expected', () => {
    expect(render(<MyComponent />))
      .toMatchSnapshot();
  });
});
```

## Enzyme mock event

```js
  describe('onChange', () => {
    let myComponent;
    let onChange;

    beforeEach(() => {
      onChange = jest.fn();
      myComponent = mount(
        <MyComponent
          onChange={ onChange }
        />
      );
      act(() => {
        myComponent
          .find('input[type="file"]')
          .first()
          .simulate('change');
      });
    });

    it('calls the callback', () => {
      expect(onChange)
        .toHaveBeenCalled();
    });
  });
  ```


