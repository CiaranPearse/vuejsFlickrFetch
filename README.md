# flickr-vuejs

> A Vue project to fetch the latest photos from Flickr based on tag input in batches of 20. When the user scrolls to the bottom of the page it loads the next 20 photos.

## Frontend Template
I've used Vuetify as a frontend Template. The main reasons for this were for the grid, input fields and cards. Why reinvent the wheel?

I;ve decided to go with the Dark version of the template. Theres no real reason for this other than I want it to be the "one thats just that little bit different"


## Infinate Scroll
I've used epic-spinners (https://github.com/epicmaxco/epic-spinners) for the loading cues as they give a nice visual. I have set a timeout (set to 3000 ms) on the fectches solely to give a visual cue that more images are loading. This spinner, in reallity isnt need as the results from the calls are near instantanious. However, it is a nice visual cue that something is happening in the background.



## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```


``` keys
#Flickr key: 6347d99644f5a2b060c5559e70ecbfbd
Secret: 2fb19b4fa63ae00d
```