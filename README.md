### flaskapi
---
http://www.flaskapi.org/

```py
from flask import request, url_for
from flask_api import FlaskAPI, status, exceptions

app = FlaskAPI(__name__)

notes = {
  0: 'do the shopping',
  1: 'build the codez',
  2: 'paint the door',
}

def note_repr(key):
  return {
    'url': request.host_url.rstrip('/') + url_for('notes_detail', key=key),
    'text': notes[key]
  }

@app.route("/", methods=['GET', 'POST'])
def notes_list():
  """
  """
  if request.method == 'POST':
    note = str(request.data.get('text', ''))
    idx = max(notes.keys()) + 1
    notes[idx] = note
    return note_repr(idx), status.HTTP_201_CREATED
    
  return [note_repr(idx) for idx in sorted(notes.keys())]

@app.route("/<int:key>/", methods=['GET', 'PUT', 'DELETE'])
def notes_detail(key):
  """
  """
  if request.method == 'PUT':
    note = str(request.data.get('text', ''))
    notes[key] = note
    return note_repr(key)

  elif request.method == 'DELETE':
    notes.pop(key, None)
    return '', status.HTTP_204_NO_CONTENT
    
  if key not in notes:
    raise exceptions.NotFound()
  return note_repr(key)

if __name__ == "__main__":
  app.run(debug=True)
  



@app.route('/example/')
def example():
  return {'requet data': request.data}

@app.route('/example/')
def example():
  return {'hello': 'word'}
  
from flask_api import FlaskAPI
app = FlaskAPI(__name__)



class PlainTextParser(BaseParser):
  """
  """
  media_type = 'text/plain'
  def parse(self, stream, media_type, **options):
    """
    """
    return stream.read().decode('utf8')
 
app.config['DEFAULT_PARSERS'] = [
  'flask.ext.api.parsers.JSONParser',
  'flask.ext.api.parsers.URLEncodedParser',
  'flask.ext.api.parsers.MultiPartParser'
]

from flastk.ext.api.decorators import set_parsers
from flask.ext.api.parsers import JSONParser

@app.route('/example_view/')
@set_parsers(JSONParser, MyCustomXMLParser)
def example():
  return {
    'example': 'Setting renderers on per-view basis',
    'request data': request.data
  }
```

```sh
python ./example.py
curl -X GET http://127.0.0.1:5000/
curl -X GET http://127.0.0.1:5000/1/
curl -x PUT http://127.0.0.1:5000/1/-d text="flask api is teh awesomez"

pip install Flask-API
```

```
HTTP 200 OK
Content-Type application/json

{
  "url": "http://127.0.0.1:5000/1/",
  "text": "build the codez"
}
```
