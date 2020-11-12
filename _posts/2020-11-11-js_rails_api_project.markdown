---
layout: post
title:      "JS/Rails API project"
date:       2020-11-12 01:24:33 +0000
permalink:  js_rails_api_project
---


### The plan

For my Javascript / Rails API project, I knew I wanted to build something different from my previous projects. I was also hyper-focused on creating a solid MVP, not only because it's good practice, but because I wanted to focus on the structure of the app and ensure I felt comfortable getting the scoffolding in place before building on top of that. 

Since the pandemic began, I've been more intentional about shopping locally in order to support my community. Building off of that idea and wanting to support the queer community in my town, I chose to create an app that would help folks find local businesses that are owned by queer people. The app allows the user to search for a business by name or category. The user is also able to create a business, if the one they're looking for isn't found.

### Bootstrap and modals
I found Ayana's JS project build videos (pinned in the "js-project-build" slack channel) a SUPER helpful reference for getting my API set up and establishing the framework for my frontend.  Since I wanted to get to an MVP pretty quickly, I decided to use Bootstrap to frame and style my frontend. It's pretty quick to get set up. The challenges came when I needed to connect the data from my API to what my app was showing in Bootstrap modals. I wanted to use modals in place of additional pages to show content because a) I wanted the app to be super slim and feel to the user like they're able to interact with it without having to click around to different pages and b) I intentionally don't show a lot of content, so having an entirely new page felt unnecessary.

Back to the Bootstrap modals -- I spent a good bit of time combing through their docs: https://getbootstrap.com/docs/4.0/components/modal/ and browsing stackoverflow. There were three "snags" I ran into in getting the modals and my data to work the way I wanted: 1) targeting the search bar, capturing that searched value to pass to my fetch request and then passing the fetched data to my modal to show the user (code 1.1), 2) adding category buttons and essentially doing something similar to #1 (code 1.2), and 3) clearing the modal at the proper time (code 1.3).

code 1.1
```
  // fetch business from user inputted search; listen to search bar submit
    const searchBar = document.querySelector('#mySearch')
    searchBar.addEventListener('click', e => {
    })

    searchBar.addEventListener('change', e => {
      submitSearch(e.target.value);
    })

    function submitSearch(searchValue) {
      searchValue = searchValue.toLowerCase()
    fetch(endPoint)
    .then(r => r.json())
    .then(businesses => {
      const searchedBusiness = businesses.data.find(b => {
          if (b.attributes.name.toLowerCase() === searchValue) {
            return true
          }
        })
        showBusiness(searchedBusiness);
      })
    }

    // shows modal with found business data
    function showBusiness(b) {
      let data = b
      $("#my_modal .modal-header").html(`<h4>${data.attributes.name}</h4>`)
      $("#my_modal .modal-body").html(
        `
          <p>${data.attributes.description}</p>
          <p>${data.attributes.category.name}</p>
          <a href="${data.attributes.website}" class="btn btn-primary">${data.attributes.website}</a>
        `
      );
      $("#my_modal").modal("show")
      $("#mySearch").val('')
      };

```

code 1.2
```
    // event listener for category search buttons
      // listen for all category buttons. When one is selected, grab that div id and pass to fetch (not sure on this last part)
      const categoryButton = document.querySelector('#category-buttons')
        categoryButton.addEventListener('click', e => {
          const categorySelected = e.target.textContent

          getBusinessByCategory(categorySelected);
      })

      // fetch to grab all businesses
      function getBusinessByCategory(selectedCategory) {
        fetch(endPoint)
        .then(r => r.json())
        .then(businesses => {

          // find all businesses that match the passed-in category and send to a function to display
          // searchedCategory will return an array of objects (since filter returns an array)
          const searchedCategory = businesses.data.filter(b => b.attributes.category.name === selectedCategory)
                displayBusinessesByCategory(searchedCategory)
            })
        }

        function displayBusinessesByCategory(newCategoryArray) {
        // pass this new array through forEach to display in the modal
          const newArray = []
          newCategoryArray.forEach(b => {
            newArray.push(`
              <h2>${b.attributes.name}</h2>
              <p>${b.attributes.description}</p>
              <a href="${b.attributes.website}" class="btn btn-primary">${b.attributes.website}</a><br><br>
            `)
          })
          $(".category-modal-body").html(newArray)
          $("#category-modal").modal("show");
        }
```

code 1.3
```
     $("#my_modal .modal-header").html("");
     $("#my_modal .modal-body").html("");
```

Hopefully these examples help someone else out there!



