# igorm

Interface wrapper for gorm.DB

"This is an interface-equivalent of gorm.DB so that those who prefer mocking gorm.DB under test can easily do so. It also adds 2 functions Wrap() and Openw(), to facilitate creating gorm.Gormw instances."

### Acknowledgments

This code was created entirely by [littledot](https://github.com/littledot)

My only contribution is to export to the external package and create documentation with an example

### Why this is not in the gorm package?

[Jinzhu](https://github.com/jinzhu), the creator of [gorm](https://github.com/jinzhu/gorm) claims that he is not a fan of mock testing without real database and does not want to change his package (see https://github.com/jinzhu/gorm/pull/805). That's why [littledot](https://github.com/littledot) introduced a simple, parallel interface that does not interfere strongly into the main package. [Jinzhu](https://github.com/jinzhu), however, did not implement this function in the gorm, so the igorm package was created (see https://github.com/jinzhu/gorm/pull/1424).

In my opinion, it is always good to have additional options, in this case mock testing.
 
### Example

First, create or change your existing functions to not use *gorm.DB but instead, use igorm.Gormw interface.

```go
func getUser(db igorm.Gormw) *User {
}
```

From now on you are not forced to use a *gorm.DB instance, but you can use an instance of any structure that implements all methods from the Gormw interface. That structure is the wrapper available in this package. You can obtain it with Openw() function.

```go
db, err := igorm.Openw(dialect, path)
if err != nil {
    log.Fatal(err)
}
```

And now it's possible.

```go
user := getUser(db)
```

Of course, for mock testing or for other purposes, you can create your own structure implementing the Gormw interface (which has all its methods) and implement these methods depending on your needs. Then use it as an value to igorm.Gormw type arguements.

### Note about interfaces and pointers

Gorm is ussually used as *gorm.DB (pointer to database instance). For interfaces use values. See explanation at this [link](https://medium.com/@agileseeker/go-interfaces-pointers-4d1d98d5c9c6)

```go
// NOT

func getUser(db *igorm.Gormw) *User {
}
```

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
