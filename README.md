# nlp_models

These natural language models are the models provided with ATLAS.ti

##Geting started
The models are built using spaCy which is free: www.spacy.io
spaCy needs python

You need to unzip the file an then pass the read data to a method like this:
```
DATA_KEY = "data"
LANG_KEY = "lang"
PIPELINE_KEY = "pipeline"
CHECKSUM_KEY = "checksum"

def deserialize_nlp(data):
    try:
        model = pickle.loads(data)
    except Exception as exception:
        logging.error(f"Could not load, reason {exception}")
        return None

    data_bytes = model[DATA_KEY]
    expected_checksum = sha_hexhash(data_bytes)
    if model[CHECKSUM_KEY] != expected_checksum:
        logging.fatal(f"Invalid checksum, got {model[CHECKSUM_KEY]}, expected {expected_checksum}.")
        return None

    nlp = spacy.blank(model[LANG_KEY])
    for pipe_name in model[PIPELINE_KEY]:
        pipe = nlp.create_pipe(pipe_name)
        nlp.add_pipe(pipe)
    nlp.from_bytes(data_bytes)

    return nlp
```
See documentation here: https://spacy.io/usage/saving-loading


Get the pipes with: nlp.pipe_names
Get single pipes with nlp.get_pipe("pipe_name")

Check out: https://spacy.io/usage/linguistic-features for instructions on how to work with spaCy
