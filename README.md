# back-end-e-commerce-shop writable on Express with MongoDB

## Project setup
```
npm install
```

## Next your need to install one mongoDB atlas account. Create cluster and one user. This user neeeded because we will need a userId in future. Then you need to refactor in index js url adress in variable "url". Nessesary: You need to add a your unique passward in fields passward! This paswward you will get when you registration user in mongoDB Atlas

```
async function start() {
  try {
    const url = `mongodb+srv://Vladislav:<password>@back-end-e-commerce-sho.dicb6.mongodb.net/shop`
    await mongoose.connect(url, {
      useNewUrlParser: true,
      useFindAndModify: false
    })
```
## Then change unique "id" in input parameter for your userId.
```
app.use(async (req, res, next) => {
  try {
    const user = await User.findById('606dd119ab8c103b00686091')
    req.user = user
    next()
  } catch (e) {
    console.log(e)
  }
})
```

## You can also change the fields "email" and "name" for your comfortable but this not necessarily.
```
const candidate = await User.findOne()
    if (!candidate) {
      const user = new User({
        email: 'pestsov.js@mail.ru',
        name: 'Vlad',
        cart: {items: []}
      })
      await user.save()
    }
```

## At future I will add a container with mongoDB in docker.

