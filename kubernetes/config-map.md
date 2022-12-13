## ConfigMaps and Secrets

**Configuring Containerized Application**
- Container images are build to be "portable"
- Containers expect configuration from
    - Configuration files
    - Command line arguments
    - Environment variables
- INI - XML - JSON - Custom Format

**ConfigMap**
- Decouples configuration from pods and components
- Stores configuration data as key-value pairs
    - Configuration files 
    - Command line arguments
    - Environment variables
- Similar to Secrets but don't contain sensitive information
- Must create a ConfigMap before referencing it in a Pod spec

**Creating ConfigMaps**
- `$ kubectl create configmap <map-name> <data-source>`

For `<data-source>`:
- Path to dir/file: `--from-file`
- Key-Value Pair: `--from-literal`

