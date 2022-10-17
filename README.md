# First step

1. Install development tools
2. Create Angular App
  a. Create project's folder
  b. Install @angular/cli (npm i -g @angular/cli)
  c. Create App as frontend (ng new frontend --skip-tests)
3. Add Header (cd frontend/ng g c components/partials/header)
  a. Generate Component
  b. Add html
  c. Add css
4. List Foods
  a. Create Food model (src/app/shared/models/Food.ts)
  b. Create data.ts
    a. Add sample foods
  c. Add images to assets
  d. Create Food service (ng g s services/food)
  e. Create Home component (ng g c components/pages/home)
    a. Add ts
    b. Add html
       1. src\app\components\pages\home\home.component.html)
       2. (+ in src\app\app.component.html add < app-home >)
    c. add ❤ and stars -> npm install ng-starrating --force
    d. in file src\app\app.module.ts in section imports:[] add RatingModule, 🕒 class 'cook-time' and 'price'
    e. Add css in src\app\components\pages\home\home.component.css
5. Search
    1. Add method to Food service
    2. Add search route (in file app-routing.module.ts after {path:'', component:HomeComponent}, add->  {path:'search/:searchTerm', component:HomeComponent})
    3. Show search result in Home component
    4. Generate search component (ng g c components/partials/search)
       1. Add to home component
       2. Add ts
       3. Add html
       4. Add css
6. Tags Bar
   1. Create Tag.ts model (in app/shared/models)
   2. Add sample tags to data.ts
   3. Food service:
     a. Add get all tags method
     b. Add get all foods by tag method
   4. Add tags route (in file app-routing.module.ts after {path: 'search/...}, add->{path:'tag/:tag', component:HomeComponent})
   5. Show tag result in Home component
   6. Generate Tags component
      1. Add to home component (ng g c components/partials/tags)
      2. Add ts
      3. Add html
      4. Add css
7. Food Page
   1. Add method to food service
   2. Generate Food Page component (ng g c components/pages/food-page)
      1. Add Route
      2. Add ts
      3. Add html
      4. Add css
8. Cart Page
   1. Create CartItem Model
   2. Create Cart Model
   3. Generate Cart service (ng g s services/cart)
   4. Add to Cart Button in Food Page
   5. Generate Cart page component (ng g c components/pages/cart-page)
      1. Add Route
      2. Add ts
      3. Add html (ng g c components/partials/title)
      4. Add css
9. Not Found!
   1. Generate Component (ng g c components/partials/not-found)
      1. Add ts
      2. Add html
      3. Add css
   2. Add To Pages
      1. Home Page
      2. Food Page
      3. Cart Page
10. Connect To Backend
    1. Create backend folder
    2. npm init -y
    3. npm install typescript
    4. Create tsconfig.json
    5. Create .gitignore
    6. Copy data.ts to backend/src
    7. npm install express cors
    8. in backend/src add new file: server.ts
    9. Create server.ts
       1. in server.ts import express and cors, const port= 5000, app.get('/api/foods', (req, res) => {
      res.send('Hello')})
       2. npm install ts-node --save-dev
       3. npm install nodemon ts-node --save-dev
       4. in package.json add "scripts": {
        "start": "cd src && nodemon server.ts",...}
11. Add url to new file: frontend/constants/urls.ts
12. In frontend in app.module.ts add HttpClient module: import {HttpClientModule} from '@angular/common/http' and in 'imports:' add ", HttpClientModule"
13. In frontend needs to update files:
    1. 'food.service.ts' - export class FoodService { constructor(private http:HttpClient) and ' getAll(): Observable<Food[]> {
    return this.http.get<Food[]>
    (FOODS_URL) }' and change another get methods(FOODS_BY_SEARCH_URL, FOODS_BY_TAG_URL, FOODS_TAGS_URL, FOOD_BY_ID_URL).
    2. '/components/home/home.component.ts'
    in constructor add: 'let foodsObservable:Observable<Food[]>', 3 'this.foods' change to 'foodsObservable' and add: 'foodsObservable.subscribe((serverFoods) => {this.foods = serverFoods})'.
    3. 'components/food-page/food-page.component.ts': need to change 'this.food = foodService.getFoodById(params.id)' to 'foodService.getFoodById(params.id).subscribe(serverFood => {this.food = serverFood})'.
    4. 'components/partials/tags/tags.component.ts': 'this.tags = foodService.getAllTags()' change to 'foodService.getAllTags().subscribe(serverTags => {this.tags = serverTags})'.
14. Login Page
    1. Generate Component (cd frontend; in terminal: ng g c components/pages/login-page)
       1. Add to routes: in app-routing.module.ts add: '{path:'login', component: LoginPageComponent}'
       2. Add ts
       3. Add html
          1. Import Reactive Forms Module in app.module.ts add in imports: 'ReactiveFormModule'
       4. Add Css
    2. Add Login API.
       1. In backend/src/server.ts after app.get method for foodId add: ' app.post('/api/users/login', (req,res) => { const {email, password} = req.body
       const user = sample_users.find(user => user.email === email && user.password === password)
       if(user){
       res.send(generateTokenResponse(user))
       } else{
       res.status(400).send("User name or password is not valid!")}}) '
       2. Use json. In backend/src/server.ts in upp after const app = express() we add: 'app.use(express.json())'
       3. Add jsonwebtoken.
          1. In backend/src/data.ts add: 'export const sample_users: any[] = [{name, email,password, address,isAdmin}].
          2. In server.ts in down before const port = 5000 add:
             1. 'app.post('/api/users/login', (req,res) => {const {email, password} = req.body
             const user = sample_users.find(user => user.email === email && user.password === password)
             if(user){res.send()}})'
             2. 'const generateTokenResponse = (user:any) => {const token = jwt.sign({email:user.email, isAdmin:user.isAdmin},"SomeRandomText", {expiresIn:"30d"})
              user.token = token
              return user}'
    3. In backend in terminal: ' npm install jsonwebtoken' and in server on top 'import jwt from "jsonwebtoken" '
    4. Test Using Postman
15. Generate User Service
    1. Generate User model in frontend -->
     (ng g s services/user)
    2. Add User Subject
    3. Add Login Method
       1. Add User Urls
       2. Generate IUserLogin interface
       3. Add ngx-toastr -> npm install ngx-toastr --force
          1. Import {ToastrModule} in app.module.ts and
          2. Import {BrowserAnimationsModule} + in 'imports: [..., BrowserAnimationsModule,...ToastrModule]
          3. Add styles in angular.json -> "styles": [..., "node_modules/ngx-toastr/toastr.css"],
          4. Add to Header:
             1. Add Local Storage methods
             2. Add Logout Method
16. Make Components for Login Page
    1. Input Container in frontend: 'ng g c components/partials/input-container'
    2. Input Validation - 'ng g c components/partials/input-validation'
    3. Text Input - 'ng g c components/partials/text-input'
    4. Default Button - 'ng g c components/partials/default-button'
17. Connect Login API To MongoDB Atlas
    1. Moving Apis into routers
    2. Create MongoDB Atlas
    3. Create .env file
    4. Install (npm install mongoose dotenv bcryptjs jsonwebtoken express-async-handler)
       1. mongoose
       2. dotenv
       3. bcryptjs
       4. jsonwebtoken
       5. express-async-handler
    5. Connect to MongoDB Atlas
    6. Use MongoDB instead of data.ts in apis
18. Register User
    1. Add Register api
    2. Add Register service method
    3. Add Register link
    4. Add Register Component (ng g c components/pages/register-page)
19. Loading! (cd frontend: ng g c components/partials/loading)
    1. Add Image (loading.svg)
    2. Add Component
    3. Add Service (ng g s services/loading)
    4. Add Interceptor (cd frontend: ng g interceptor shared/interceptors/loading)
20. Checkout Page
    1. Create Order Model (in src/app/shared/models create file Order.ts)
    2. Create Checkout Page Component (ng g c components/pages/checkout-page)
       1. Add To Router
    3. Add User to User Service
    4. Add Cart to Cart Service
    5. Create Order Items List Component (ng g c components/partials/order-items-list)
