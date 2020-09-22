# React Testing
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
## API call
```js
describe('my service', () => {
  describe('list()', () => {
    describe('and the API call succeeds', () => {
      let myService;

      beforeEach(async () => {
        fetchMock.get(
          config.api.myService.url,
          {
            status: statusCodes.ok,
            body: [
              {
                id: 1,
                label: 'Hello',
            ]
          }
        );

        result = await myService.list();
      });

      afterEach(() => {
        fetchMock.restore();
      });

      it('calls the API', () => {
        expect(fetchMock.called())
          .toBe(true);
      });

      it('resolves with JSON data', () => {
        expect(result)
          .toContainEqual({
            id: 1,
            label: 'Hello'
          });
      });
    });

    describe('and the API call fails', () => {
      beforeAll(() => {
        fetchMock.get(
          config.api.myService.url,
          {
            status: statusCodes.internalServerError
          }
        );
      });

      afterAll(() => {
        fetchMock.restore();
      });

      it('rejects with status', () => {
        expect(myService.list())
          .rejects
          .toThrow('Failing to fetch expertises 500: Internal Server Error');
      });

      it('calls the API', () => {
        expect(fetchMock.called()).toBe(true);
      });
    });
  });
});
```

