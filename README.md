# Set flash message for routes.

This package is build to send the flash messages on the top of Gofiber

## Installation
The package can be used to validate the data and send flash message to other route.
> go get github.com/usepzaka/fiberflash


## Usage

```go
package main

import (
	"github.com/gofiber/fiber/v2"
	"github.com/usepzaka/fiberflash"
)

func main() {
	app := fiber.New()
	app.Get("/success-redirect", func (c *fiber.Ctx) error {
		return c.JSON(fiberflash.Get(c))
	})

	app.Get("/error-redirect", func (c *fiber.Ctx) error {
		fiberflash.Get(c)
		return c.JSON(fiberflash.Get(c))
	})

	app.Get("/error", func (c *fiber.Ctx) error {
		mp := fiber.Map{
			"error": true,
			"message": "I'm receiving error with inline error data",
		}
		return fiberflash.WithError(c, mp).Redirect("/error-redirect")
	})

	app.Get("/success", func (c *fiber.Ctx) error {
		mp := fiber.Map{
			"success": true,
			"message": "I'm receiving success with inline success data",
		}
		return fiberflash.WithSuccess(c, mp).Redirect("/success-redirect")
	})

	app.Listen(":8080")
}

```