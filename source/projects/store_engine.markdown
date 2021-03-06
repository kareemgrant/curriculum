---
layout: page
title: StoreEngine
---

In this project you'll use Ruby on Rails to build an online commerce platform.

<div class="note">
<p>The project deadline is Thursday, 4/19 at 1:00PM.</p>
</div>

### Learning Goals

* Use TDD to drive all layers of Rails development including unit, integration, and user acceptance tests
* Practice mixing HTML, CSS, and Rails templates to create an inviting and usable User Interface
* Differentiate responsibilities between components of the Rails stack

### Understandings

Please consider the requirements below non-exhaustive guidelines for building a great store. If you know something should be done but it isn't listed below, do it.

### Restrictions

Project implementation may *not* use:

* Devise for Authentication

### Base Expectations

You are to build an online store which offers both administrator and public shopper interfaces.

#### Administrators

As an authenticated Administrator, I can:

* Create product listings including a title, description, price, and a photo
* Modify existing products title, description, price, and photo
* Create named categories for products
* Assign products to categories or remove them from categories. Products can belong to more than one category.
* Retire a product from being sold, which hides it from browsing by any non-administrator

As an Administrator, I can also view an order "dashboard" where I can:

* See a listing of all orders with:
    * the total number of orders by status
    * links for each individual order
    * filter orders to display by status type (for statuses "pending", "cancelled", "paid", "shipped", "returned")
    * link to transition to a different status:
        * link to "cancel" orders which are currently "pending"
        * link to "mark as returned" orders which are currently "shipped"
        * link to "mark as shipped" orders which are currently "paid"
* Access details of an individual order, including:
    * Order date and time
    * Purchaser full name and email address
    * For each product on the order
        * Name with link to product page
        * Quantity
        * Price
        * Line item subtotal
  * Total for the order
  * Status of the order
  * Links to transition to other statuses as explained above

#### Shoppers

As a Public User, I can:

* Browse all products
* Browse products by category
* Add a product to my cart
* View my cart
* Remove a product from my cart
* Increase the quantity of a product in my cart
* Checkout by entering my billing information, shipping address, and email address

### Security and Usability

#### Unauthenticated Users

Allowed To:

* browse items or categories
* add items to a cart
* log in which should _not_ clear the cart

NOT Allowed To:

* view another public users's private data (such as current shopping cart, etc.)
* checkout (until they log in)
* view the administrator screens or use administrator functionality
* make themselves an administrator

#### Authenticated Non-Administrators

Allowed To:

* do everything Unauthenticated Users can do except "log in"
* log out
* view their past orders with links to display each order
* on that order display page there are:
  * products with quantity ordered and line-item subtotals
  * links to each product page
  * the current status
  * order total price
  * date/time order was submitted
  * if shipped or cancelled, display a timestamp when that action took place
  * if any product is retired:
      * they can still access the product page
      * they cannot add it to a new cart
* Place a "Two-Click" order from any active product page. The first click asks "Place an order for 'X'?" and if you then click "OK", the order is completed. Handle this in JavaScript or plain HTML at your discretion.

NOT Allowed To:

* view another public users's private data (such as current shopping cart, etc.)
* view the administrator screens or use administrator functionality
* make themselves an administrator

#### Administrators

Allowed To:

* View, create, and edit products and categories
* View and edit orders; may change quantity or remove products from orders with the status of pending or paid
* Change the status of an order according to the rules as outlined above

NOT Allowed To:

* Modify public user personal data

### Data Validity

There are several types of entities in the system, each with requirements about what makes for a valid record. These restrictions are summarized below.

Any attempt to create/modify a record with invalid attributes should return the user to the input form with a validation error indicating the problem along with suggestions how to fix it.

#### Product

* A product must have a title, description, and price.
* The title and description cannot be empty strings.
* The title must be unique for all products in the system
* The price must be a valid decimal numeric value and greater than zero
* The photo is optional. If present it must be a valid URL format.

#### User

* A user must have a valid email address that is unique to all users
* A user must have a full name that is not blank
* A user may optionally provide a display name that must be no less than 2 characters long and no more than 32

#### Order

* An order must belong to a user
* An order must be for one or more of one or more products currently being sold

### Example Data

To support the evaluation process, please make the following available via the `rake db:seed` task in your production deployment:

* Products
  * At least 20 products of varying prices
  * Some of the products should be attached to multiple categories
* Categories
  * At least 5 categories with a varying number of member products
* Orders
  * At least 10 sample orders, with at least two at each stage of fulfillment (`pending`, `shipped`, etc)
* Users
  * Normal user with full name "Matt Yoho", email address "demoXX+matt@jumpstartlab.com", password of "hungry" and no display name
  * Normal user with full name "Jeff", email address "demoXX+jeff@jumpstartlab.com", password of "hungry" and display name "j3"
  * User with admin priviliges with full name "Chad Fowler", email address "demoXX+chad@jumpstartlab.com", password of "hungry", and display name "SaxPlayer"

### Submission Guidelines

Your project must be "live" on the web for your peers to evaluate it. Setup your own VPS, use Heroku, whatever you have to do to make it work.

Your `README` file on Github should contain a link to your live site.

Conversely, on the live site, setup the URL http://yourwebsite.com/code to redirect the user to the Github repository.

### Extensions

#### Put Items on Sale

Administrators may put products or entire categories of products on sale. They can:

* Create a "sale" and connect it with individual products or entire categories
* Sales are created as a percentage off the normal price
* View a list of all active sales
* End a sale

On the order "dashboard" they can:

* View details of an individual order, including:
    * If purchased on sale, sale percentage and adjusted price
    * Total for the order, including any discount from applicable sales

As a Public User:

* Sale prices are displayed in product listings alongside normal price and percentage savings

#### Product Reviews

On any product I can, as a Public User:

* See the posted reviews including:
    * title, body, and a star rating 0-5
    * the display name of the reviewer
* See an average of the ratings broken down to half-stars

On products I've purchased I can:

* Add a rating including:
    * Star rating 0-5
    * Title
    * Body text
* Edit a review I've previously submitted until 15 minutes after I first submitted it

#### Search

Implement search for both the consumer and administrator:

* Consumer
    * Search for products in the whole site
    * Search through "My Orders" for matches in the item name or description
* Administrator
    * Search orders using a builder-style interface (like Google's "Advanced Search") allowing them to specify any of these:
        * Status (drop-down)
        * Order total (drop-down for `>`, `<`, `=` and a text field for dollar-with-cents)
        * Order date  (drop-down for `>`, `<`, `=` and a text field for a date)
        * Email address of purchaser

#### Transaction Processor

Implement a "checkout" procedure using Stripe, Paypal or another service to handle credit card transactions in a "sandboxed" developer environment.

When the card is processed, update the order to "paid" and send a confirmation email to the user. Emails should _only_ be sent when the app is in `production` mode. Don't spam people while you're getting it working.

### Evaluation Criteria

This project will be peer assessed using user-driven stories and the rubric below. 

1. Correctness
  * 3: All provided stories pass 
  * 2: One story could not be completed
  * 1: Two or three stories could not be completed
  * 0: More than three stories could not be completed
2. Testing
  * 3: Testing suite covers >95% of application code
  * 2: Testing suite covers 85-94% of application code
  * 1: Testing suite covers 70-84% of application code
  * 0: Testing suite covers <70% of application code
3. Code Style
  * 3: Source code generates no complaints from Cane or Reek
  * 2: Source code generates three or fewer warnings 
  * 1: Source code generates four to eight warnings
  * 0: Source code generates more than eight warnings
4. Live Hungry
  * 4: Program fulfills all Base Expectations and four Extensions
  * 3: Program fulfills all Base Expectations and two Extensions
  * 2: Program fulfills all Base Expectations
  * 1: Program is missing 1-3 features from the Base Expectations
  * 0: Program is missing more than three features from the Base Expectations
5. User Interface & Design
  * 3: WOW! This site is beautiful, functional, and clear
  * 2: Very good design and UI that shows work far beyond dropping in a library or Bootstrap
  * 1: Some basic design work, but doesn't show much effort beyond "ready to go" components/frameworks/etc
  * 0: The lack of design makes the site difficult / confusing to use
6. Surprise & Delight
  * 2: A great idea that's well executed and enhances the shopping experience
  * 1: An extra feature that makes things a little nicer, but doesn't blow your mind
  * 0: No surprise. Sad face :(

### Evaluation Stories

Pull the stories from the upstream project which you originally forked and look in `/user_stories`. You can edit the `<>` markers to match with the theme/contents of your store.

### Code Style Metrics

This is all tested in Ruby 1.9.3 which is the expected platform for your project.

#### Setup

In your project's Gemfile, you must add these two dependencies:

```
  gem 'reek', :git => "git://github.com/mvz/reek.git", :branch => "ripper_ruby_parser-2"
  gem 'cane', :git => "git://github.com/square/cane.git"
```

#### Running Reek

```
bundle exec reek app/**/*.rb | grep "TooManyStatements\|UncommunicativeVariableName\|LongMethod"
```

#### Running Cane

```
bundle exec cane --style-glob 'app/**/*.rb' --abc-glob 'app/**/*.rb' --no-doc
```

#### Reading Results

See the rubric section "Code Style" above.

### Evaluation Protocol

Projects will be evaluated the afternoon of Thursday, 4/19.

* *1:00 - 2:15* Round of Fours
* *2:20 - 3:10* Final Four Presentations
* *3:10 - 3:20* Final Scoring
* *3:20 - 3:50* Surprise Showcase
* *3:50 - 4:00* Champion Announced
* *4:00 - 4:30* Wrapup / Retrospective

#### Round of Fours

In this round you'll break into three groups of four pairs each.

* Group 1 in the Fishbowl
  * Jacqueline Chenault & Jan Koszewski
  * Tom Kiefhaber & Chris Anderson
  * Christopher Maddox & Daniel Kaufman
* Group 2 in the Classroom "High Country"
  * Charles Strahan & Mark Tabler
  * Melanie Gilman & Austen Ito
  * Mary Cutrali & Conan Rimmer
* Group 3 in the "Extra Room"
  * Edward Weng & Michael Chlipala
  * Andrew Glass & Elise Worthy
  * Jonan Scheffler & Darrell Rivera
* Group 4 in the Classroom "Low Country"
  * Nisarg Shah & Mike Silvis
  * Horace Williams & Travis Valentine
  * Michael Verdi & Andrew Thal

Follow the following protocol:

* Each pair presents their code/project for 5 or fewer minutes. Make sure to highlight any S&D features or extensions.
* Pairs then evaluate both other projects for 20 minutes each:
  * work through the evaluation stories
  * run the code metrics
  * subjectively measure the UI and S&D categories
  * submit one evaluation per project http://eval.jumpstartlab.com (so your pair submits a total of two evals, one for each project you examine)

When all projects have been evaluated, use the total aggregate score of all sections across the two evaluations to choose *one* project to move on to the next round.

#### Final Four &trade;

Each of the champions from the first round will present to the whole group and guests. You have seven minutes to show off:

* The basics
* What makes your project exceptional?
* How did you integrate S&D?
* Anything else you're proud of?

Audience members will then be invited to try out your store for five minutes.

When all four projects have been presented, all members of the audience will then submit a ranking of the four projects to http://eval.jumpstartlab.com

#### Surprise Showcase

If you weren't in the final four, here's your chance to *quickly* show the whole group what's exceptional about your project.

#### Wrapup / Retrospective

* What was challenging about this project?
* What came easy?
* What would you have done differently?
* Did you reach your goals? Why or why not?
* Any lessons learned for the next project?
