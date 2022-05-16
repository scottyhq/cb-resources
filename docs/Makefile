test:
	docker run --rm --volume="$$PWD:/srv/jekyll:Z" -e "JEKYLL_ENV=docker" -p 4000:4000 jekyll/jekyll:4.2.0 \
	jekyll serve --force_polling --livereload --config _config.yml,_config.docker.yml
