Pod requirements:
pod: 'jekyll' has an initContainer, name: 'copy-jekyll-site', image: 'gcr.io/kodekloud/customimage/jekyll' and command: rm -rf /site/* && jekyll new /site && cd /site && bundle install


Container 'jekyll' should run the command: cd /site && bundle install && bundle exec jekyll serve --host 0.0.0.0 --port 4000


pod: 'jekyll', initContainer: 'copy-jekyll-site', mountPath = '/site'


pod: 'jekyll', initContainer: 'copy-jekyll-site', volume name = 'site'


pod: 'jekyll', container: 'jekyll', volume name = 'site'


pod: 'jekyll', container: 'jekyll', mountPath = '/site'


pod: 'jekyll', container: 'jekyll', image = 'gcr.io/kodekloud/customimage/jekyll-serve'


pod: 'jekyll', uses volume called 'site' with pvc = 'jekyll-site'


pod: 'jekyll' uses label 'run=jekyll'

PV already exists.


Testing:
curl http: http://node01:30097


