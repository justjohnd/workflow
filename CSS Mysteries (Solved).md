# Heights and Widths

1. Problem: Image overflows container with set height [(Ex. 001)](https://codepen.io/justjohnd/pen/rNwjXad).
  Reason: divs generally to not limit the data inside, such as images or texts
  
  Solutions:
  - Use box-sizing. Adding `box-sizing: border-box` to the container will prevent padding or margins from overriding the containers set dimensions. However, you also need to set the image dimension to 100% (in this example, `height: 100%`). [Ex. 003](https://codepen.io/justjohnd/pen/ExXZqrB). This example can be simplified by removing the container, and [applying the properties directly to the image](https://codepen.io/justjohnd/pen/OJgWKdg).
  
  - Use flex-box. Set the container to `display: flex`, then add margin or padding directly to the image. Flexbox will automatically adjust the image size to fit the container, and include the padding or margins. This is the most responsive solution. [Ex. 004](https://codepen.io/justjohnd/pen/JjJEgzj).

# Flexbox Columns
1. Problem: Align two items in a colunt to the top and bottom of the column.
  Solution: Flexbox. [Ex. 005](https://codepen.io/justjohnd/pen/eYRvLaQ)
    1) Set width of parent and children to be the same. You can just set the children's width to 100%
    2) On the parent add:
    ```
    .parent {
    display: flex;
    flex-wrap: wrap;
    width: [desired-width];
    height: [desired-height];
    align-content: space-between;
    }
    ```
    
    Note that flexbox default is `flex-wrap: nowrap`. Without `wrap`, the flex items will continue to be added linearly until reaching either the set width of the container or 100vw. Items will not overflow, and their own specified widths will be overridden. Item widths will continue to get smaller in order to fit inside the flexbox.

2. Problem: Align to items in a column to the left and right of the column **in a nested flexbox**.
   Solution: If you are trying to achieve this inside another nested flexbox, set the **parent width** of the current flexbox you are working in to `100%`, then apply `display: flex` and `justify-content: space-between`. [See here](https://codepen.io/justjohnd/pen/YzrWZQq). If you are not nested within a flexbox, you don't need to set width.
   
3. Problem: Make all columns the same width, based on the widest item.
  Solution: use `flex-grow: 1` on each child component. `flex-grow` asigns the remaining space in a flexbox to the child based on unitless factor given to it. For example, a flexbox with 2 items, each given `flex-grow: 1` will split the space: 1/2, 1/2. [See here](https://codepen.io/justjohnd/pen/WNZxpEz)
  
  
   
 
