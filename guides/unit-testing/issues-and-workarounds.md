# Unit testing common issues and workarounds 🧪 <!-- omit in toc -->

A cheatsheet for common issues and workarounds.

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
