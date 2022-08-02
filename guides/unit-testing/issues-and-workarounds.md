# Unit testing common issues and workarounds 🧪 <!-- omit in toc -->

A cheatsheet for common issues and workarounds.

- [Install and setup ***ng-mocks*** package](#install-and-setup-ng-mocks-package)
- [Components with change detection strategy *OnPush*](#components-with-change-detection-strategy-onpush)
- [Directives with *ngControl* dependency](#directives-with-ngcontrol-dependency)

## Install and setup ***ng-mocks*** package

Install [ng-mocks](https://www.npmjs.com/package/ng-mocks) package.

```shell
npm i -D ng-mocks
```

Setup in `src/test.ts` file:

```ts
...
import { MockInstance, ngMocks } from 'ng-mocks';

...

// auto spy
ngMocks.autoSpy('jasmine');

// auto restore for jasmine
jasmine.getEnv().addReporter({
  specDone: MockInstance.restore,
  specStarted: MockInstance.remember,
  suiteDone: MockInstance.restore,
  suiteStarted: MockInstance.remember,
});

...
```

## Components with change detection strategy *OnPush*

When working with *OnPush* components in tests, `fixture.detectChanges()` doesn't trigger change detection on underlying component's view but rather on host view.

`Option 1`: Get actual component's changeDetector.

```ts
const changeDetectorRef = fixture.componentRef.injector.get(ChangeDetectorRef);
changeDetectorRef.detectChanges();
```

`Option 2`: Disable change detection strategy *OnPush* .

```ts
beforeEach(async () => {
  await TestBed.configureTestingModule({
    declarations: [FooComponent, ...],
    imports: [...],
    providers: [...],
  })
    .overrideComponent(FooComponent, {
      set: { changeDetection: ChangeDetectionStrategy.Default },
    })
    .compileComponents();
});
```

## Directives with *ngControl* dependency

Create a mock host component.

ℹ️ An input html element will be used.

```ts
@Component({
  selector: 'app-host-mock',
  template: ` <input myDirective [(ngModel)]="myProperty" /> `,
})
class HostMockComponent {
  public myProperty: string = '';
}
```

Setup TestBed.

```ts
describe('MyDirective', () => {
  let fixture: ComponentFixture<HostMockComponent>;
  let hostElement: DebugElement;
  let directive: MyDirective;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [HostMockComponent, MyDirective],
      imports: [FormsModule], // ReactiveFormsModule
    }).compileComponents();
  });

  beforeEach(() => {
    fixture = TestBed.createComponent(HostMockComponent);

    hostElement = fixture.debugElement.query(By.directive(MyDirective));
    directive = hostElement.injector.get(MyDirective);

    fixture.detectChanges();
  });
  ...
});
```

Trigger `ngModelChange` event by dispatching an `input` event.

```ts
it('should trigger "ngModelChange"', fakeAsync(() => {
  const inputElement: HTMLInputElement = hostElement.nativeElement;

  const mockInputValue = 'mock input value';
  const expectedValue = 'expected value';

  inputElement.value = mockInputValue;

  inputElement.dispatchEvent(new Event('input'));
  fixture.detectChanges();
  tick();

  const result = inputElement.value;

  expect(result).toBe(expectedValue);
}));
```
