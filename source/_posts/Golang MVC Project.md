---
title: Golang MVC structure
date: 2020-1-15 11:25:33
tags:
- go
- web
- MVC
categories:
- golang
---
### This MVC doesn't use any golang web framework
   Use http for client and server communication, MUX for routing
1. structure:
in the G:/go-work/src/mvc-example:
* app/controllers
home.go
* app/models
types.go
* app/route
route.go
* app/helpers
* config/.env
* app/
main.go
2. modify & create:
comment on app/controllers like this:
``` bash
	// "github.com/gorilla/mux"
	// "mvc_example/app/models"
	// "encoding/json"
	// log "github.com/sirupsen/logrus"
```
create .env file that have key value pair of environment variable, and access that file using joho/godotenv package.add below key/value pair for test:
``` bash
site : "restapiexample.com"
```
Load the .env file into main function and access env variable using key 'site', that will return ‘restapiexample.com’.

3. codes
* main:
``` bash
package main

import (
	"github.com/joho/godotenv"
	log "github.com/sirupsen/logrus"
	"os"
	"mvc_example/app/route"
	"net/http"
)

func main() {
	log.Info("Application has been started on : 8080")
	err := godotenv.Load(os.ExpandEnv("$GOPATH/src/mvc_example/app/config/.env"))
	if err != nil {
		log.Fatal("Error loading .env file")
	}

	key := os.Getenv("Site")
	log.Info(key)

	//creating instance of mux
	routes := route.GetRoutes()
	http.Handle("/", routes)
	log.Fatal(http.ListenAndServe(":8080", nil))
}
```
* controllers/home.go:
``` bash
package controllers

import (
	"fmt"
	"net/http"
)

func Hello(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Hello, I am home controller!")
	w.WriteHeader(http.StatusOK)
	fmt.Fprintf(w, "Hello, I am home controller")
}
```
* models/types.go:
``` bash
package models
 
type User struct {
   Data struct {
      ID        int    `json:"id"`
      FirstName string `json:"first_name"`
      LastName  string `json:"last_name"`
      Avatar    string `json:"avatar"`
   } `json:"data"`
}
```
* route/route.go
``` bash
package route

import (
	"github.com/gorilla/mux"
	"mvc_example/app/controllers"
)

func GetRoutes() *mux.Router {
	routes := mux.NewRouter().StrictSlash(false)

	routers := routes.PathPrefix("/api/v1").Subrouter()

	routers.HandleFunc("/hello", controllers.Hello).Methods("GET")

	return routers
}
```


Access rest call using "http://localhost:8080/api/v1/hello"
