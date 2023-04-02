# Deploy to Github pages 🚀

Check official [docs](https://angular.io/guide/deployment#deploy-to-github-pages).

## Assets not found

Check the relative path for background assets and add prefix `~src`:

```css
background-image: url("~src/assets/images/my-awesome-image.png");
```

Check the relative path for image assets and add prefix `./`:

```html
<img [src]="./assets/images/my-awesome-image.png" />
```
