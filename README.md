chizerolog implements a zerolog middleware for go-chi

It is based on https://github.com/amanbolat/chi_middlewares

### Usage Example

```
func Routes(logger zerolog.Logger) *chi.Mux {
	router := chi.NewRouter()
	router.Use(render.SetContentType(render.ContentTypeJSON))
	router.Use(middleware.RedirectSlashes)
	router.Use(middleware.Recoverer)
	router.Use(chizero.LoggerMiddleware(&logger))
	router.Use(middleware.Timeout(5 * time.Second))

	router.Route("/v1", func(r chi.Router) {
		r.Mount("/api/user", userv1.Routes())
	})

	return router
}

func main() {
    zerolog.SetGlobalLevel(zerolog.DebugLevel)
	log.Logger = log.Output(zerolog.ConsoleWriter{Out: os.Stdout, TimeFormat: time.RFC3339})

	router := Routes(log.Logger)

	log.Info().Msg("Starting server...")

	err := http.ListenAndServe("localhost:8081", router)
	if err != nil {
		log.Fatal().Msg(err.Error())
	}
}
```
