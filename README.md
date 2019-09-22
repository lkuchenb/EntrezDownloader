# EntrezDownloader

## Usage

You can use the package with the defaults:

```python
from EntrezDownloader import EntrezDownloader

# Create a new downloader instance
edl = EntrezDownloader()

# 100 IDs
ids = [ f'NT_112{num}.1' for num in range(550, 650) ]

# Execute parallel efetch for the specified IDs
results, failed  = edl.efetch(db = 'nuccore', ids = ids)
```

This will generate a list of XML results returned by each batch invocation. Additionally, a list of IDs that were part of failed batches is returned.

To get more interpretable results, you can pass a function to `efetch()` that processes the results right after they are fetched. E.g you might want to use the Biopython Entrez XML parser:

```python
import io
from Bio import Entrez

results, failed = edl.efetch(
  db = 'nuccore',
  ids = ids,
  result_func = lambda xml_text : [ rec for rec in Entrez.parse(io.StringIO(xml_text)) ]
)
```

The function has to return a list. The default function is `lambda xml_text : [xml_text]`.
