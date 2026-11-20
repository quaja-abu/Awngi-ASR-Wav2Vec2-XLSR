# Decoder artifacts

Local decoder pickle files are available for inspection. Before committing them, verify:
- the package and version used to serialize them;
- the vocabulary/token mapping expected by each decoder;
- whether any embedded language-model files or paths are required;
- whether the files contain only decoder configuration/state and no restricted data;
- whether they can be safely loaded in a clean environment.

Pickle files can execute code during deserialization. Only load trusted files in a controlled environment. Prefer documenting decoder construction parameters and providing reproducible scripts where possible.
