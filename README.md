# candidate-js-live-test

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


# User Notes

### Control Panel

1. User can add new square to the canvas via "Add New Square" button
2. Squares can be arranged vertically via "Evenly Space Vertically" button

### Update Panel

#### To square/squares color 

User can change the color of a square/squares by selecting a square/squares in the canvas

1. Select a square/Squares
2. Use the dropdown "Color Picker" to pick the required color
3. Click on "Change Color" button to apply the changes

#### Redo/Undo Functions

Once the user start updating the color, the changes will get tracked. The user can perform Redo or Undo the changes as needed.

Note: The functionality to track adding new square to the canvas is not implemented. 
The App can track on the color changes.


#### Technical Notes

1. Created Button and Header as reusable components with prop validation

