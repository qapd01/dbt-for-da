# Setting Up dbt project
## Setting up python environment with uv
You need to use uv to install dbt Core and others dependencies in this project
## Install uv
macOS and Linux
```shell
curl -LsSf https://astral.sh/uv/install.sh | sh
```
For Windows
<br>
Use irm to download the script and execute it with iex:
```shell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
Or you can use pip install
```shell
pip install uv
```
uv is ready to use
```shell
uv --version
```

## Get start create project
Let's create project folder
```shell
mkdir dbt-project-for-de
cd dbt-project-for-de
```

Before install you should init uv files

```shell
uv init .
```

Creating a virtual environment
uv supports creating virtual environments, e.g., to create a virtual environment at my-name:
```shell
uv venv znapat
```

install dbt-core and dbt-postgres
```shell
uv pip install dbt-core | uv pip install dbt-postgres
```

To create your dbt project:

Make sure you have dbt Core installed and check the version using the dbt --version command:
```shell
dbt --version
```

Initiate dbt project:
```shell
dbt init dbt_project_for_de

```

Navigate into your project
```shell
cd dbt_project_for_de
```

## Setting up postgres
Create profile.yml
```shell
touch profile.yml
```

Open profile.yml file and copy this configuration to profile.yml
```yml
dbt_project_for_de:
  target: dev
  outputs:
    dev:
      type: postgres
      host: [hostname]
      user: [username]
      password: [password]
      port: [port]
      dbname: [database name] # or database instead of dbname
      schema: [dbt schema]
      threads: [optional, 1 or more]
      [keepalives_idle](#keepalives_idle): 0 # default 0, indicating the system default. See below
      connect_timeout: 10 # default 10 seconds
      [retries](#retries): 1  # default 1 retry on error/timeout when opening connections
      [search_path](#search_path): [optional, override the default postgres search_path]
      [role](#role): [optional, set the role dbt assumes when executing queries]
      [sslmode](#sslmode): [optional, set the sslmode used to connect to the database]
      [sslcert](#sslcert): [optional, set the sslcert to control the certifcate file location]
      [sslkey](#sslkey): [optional, set the sslkey to control the location of the private key]
      [sslrootcert](#sslrootcert): [optional, set the sslrootcert config value to a new file path in order to customize the file location that contain root certificates]  
```

Then paste configuration from superbase to profile.yml
and run this command for check connection:
```shell
dbt debug
```


