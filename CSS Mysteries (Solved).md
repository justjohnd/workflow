# Heights and Widths

1. Problem: Image overflows container with set height [(Ex. 001)](https://codepen.io/justjohnd/pen/rNwjXad).
  Reason: divs generally to not limit the data inside, such as images or texts
  
  Solutions:
  - Use box-sizing. Adding `box-sizing: border-box` to the container will prevent padding or margins from overriding the containers set dimensions. However, you also need to set the image dimension to 100% (in this example, `height: 100%`). [Ex. 003](https://codepen.io/justjohnd/pen/ExXZqrB). This example can be simplified by removing the container, and [applying the properties directly to the image](https://codepen.io/justjohnd/pen/OJgWKdg).
  
  - Use flex-box. Set the container to `display: flex`, then add margin or padding directly to the image. Flexbox will automatically adjust the image size to fit the container, and include the padding or margins. This is the most responsive solution. [Ex. 004](https://codepen.io/justjohnd/pen/JjJEgzj).
