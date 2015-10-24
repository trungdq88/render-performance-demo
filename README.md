# render-performance-demo
Demo project for my Render Performance speak at Techcamp Uni 2015

- Slide: http://j.mp/web-performance-slide
- Full article: http://j.mp/web-performance-trungdq88


## Demo 1

Very bad example that has < 30fps

```css
    .bird {
        width: 50px;
        height: 50px;
        border-radius: 25px;
        box-shadow: 20px 20px 60px rgba(0, 0, 0, 1);
        position: absolute;
        background: lightpink;
        top: 0;
        left: 0;
        transition: top 2s linear, left 2s linear;
    }
```

```js
    // This method moves the birds
    function fly($bird) {
        $bird.css({
            top: Math.round(Math.random() * 450),
            left: Math.round(Math.random() * 450)
        });
    }
```

## Demo 2

Improve by create separate composite layer for each moving elements

```css
    .bird {
        width: 50px;
        height: 50px;
        border-radius: 25px;
        box-shadow: 20px 20px 60px rgba(0, 0, 0, 1);
        position: absolute;
        background: lightpink;
        top: 0;
        left: 0;
        transition: top 2s linear, left 2s linear;
        // These 2 lines will force browser to create new composite layer for .bird
        will-change: transform;
        transform: translateZ(0); // For unsupported browsers
    }
```

## Demo 3

More improve: use `transform: translate3d(...)` to move the element instead of `top` and `left`. This will avoid trigger layout.

```css
    .bird {
        width: 50px;
        height: 50px;
        border-radius: 25px;
        box-shadow: 20px 20px 60px rgba(0, 0, 0, 1);
        position: absolute;
        background: lightpink;
        // Use transform: translate3d instead of top, left to move elements
        transform: translate3d(0, 0, 0);
        transition: transform 2s linear;
        will-change: transform;
    }
```

```js
    // This method moves the birds
    function fly($bird) {
        var newPosition = {
            top: Math.round(Math.random() * 450),
            left: Math.round(Math.random() * 450)
        };
        $bird.css({
            transform: 'translate3d(' + newPosition.top + 'px, ' + newPosition.left + 'px, 0)',
        });
    }
```

## Demo 4

More improve: use `requestAnimationFrame` for UI update, this will reduce frame drops.

```js
    // This method moves the birds
    function fly($bird) {
        var newPosition = {
            top: Math.round(Math.random() * 450),
            left: Math.round(Math.random() * 450)
        };
        requestAnimationFrame(function () {
            $bird.css({
                transform: 'translate3d(' + newPosition.top + 'px, ' + newPosition.left + 'px, 0)',
            });
        })
    }
```
