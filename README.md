# SLIMTutorial2024
Tutorials for 2024 ML4Seismic meeting

## Step by step guide

1. Copy and paste the IP address `34.123.253.131` to a brower.
2. Enter your user name and create a password the first time you log in.
3. All tutorials are saved under the directory  `/opt/ml4seismic_tutorial` (read only). Each user should make a personal copy of the tutorial directory to the home directory to work with:
  ```bash
  cp -r /opt/ml4seismic_tutorial/ ~/ml4seismic_tutorial/
  ```
  Each tutorial has its own environment files `Project.toml` and `Manifest.toml`. 
   ```bash 
   /opt/ml4seismic_tutorial/
   ├── tutorial_1/
   │   ├── Project.toml
   │   ├── Manifest.toml
   │   └── tutorial_notebook.ipynb
   ├── tutorial_2/
   │   ├── Project.toml
   │   ├── Manifest.toml
   │   └── tutorial_notebook.ipynb
   ```
4. Open a terminal and run `julia -e 'using Pkg; Pkg.add("IJulia")`'. This installs a Julia kernel to the Jupyter notebook. <p style="color:red;">**TODO**: Can I do this for everyone?</p>
5. `cd` into the directory of one of the tutorial and run the following commands to build the environment.
  ```julia
  using Pkg
  Pkg.activate(".")
  Pkg.instantiate() # Install the dependencies
  Pkg.add(["Dependency1", "Dependency2", ...]) # Install more packages if necessary
  ```




## Notes on Jupyterhub setup

### General

1. `Kubernetes` is too much. Use `The Littlest Jupyterhub`.
2. Be aware sometimes Jupyer notebooks on JupyterHub needs to restart itself.

### Install Julia/Juliaup

Add path of Julia or Juliap to `/etc/profile`, which is a system-wide configuration file that is executed when any user opens a new login shell. 
```bash
echo 'export PATH="/opt/juliaup/bin:$PATH"' | sudo tee -a /etc/profile
```
To edit a system-level file like this later, you might need `sudo vim /etc/profile`.

Another way is to make a symlink `sudo ln -s /opt/julia/bin/julia /usr/local/bin/julia`. `/usr/local/bin/` is a shared directory. It typically holds binaries intended for use by all users of the system, and it’s generally included in the default PATH for all users, but, by default, it is not in each users' `PATH`.

Permissions: Make sure the `/opt/juliaup` directory and its contents have appropriate permissions for read and execute access by all users:
```bash
sudo chmod -R 755 /opt/juliaup
```

If you install `Juliaup`, to install Julia 1.8.5 and Julia 1.10, you can run:
```bash
juliaup add 1.8.5
juliaup add 1.10
```
Users can switch between versions by running:
```bash
juliaup default 1.10
```

`JULIA_DEPOT_PATH` is an environment variable that determines where Julia looks for its package installations, environments, compiled cache, and other configuration files.
If `JULIA_DEPOT_PATH` is not set, Julia will use the default directory, typically under `~/.julia` for each user. This means that unless explicitly set, each user has their own isolated Julia package directory, which prevents package conflicts but also requires each user to install packages individually.
Setting `JULIA_DEPOT_PATH` to a shared directory allows all users to access the same packages, ensuring consistency across all environments.

### Install packages
`sudo julia -e 'using Pkg; Pkg.add("PackageName")'` installs packages for all users.

### Transfer tutorials to JupyterHub

Move tutorials to VM, `scp /path/to/tutorial.ipynb usernamen@instance-ip:~`. Replace `/path/to/tutorial.ipynb` with the path to your local file, and `instance-ip` with your Google Cloud server's IP address. 

On VM, move tutorial from your home directory to the shared folder, `sudo mv ~/tutorial.ipynb /opt/ml4seismic_tutorial/`.

Once the files are uploaded to `/opt/ml4seismic_tutorial/`, you need to make sure the permissions allow all JupyterHub users to access the directory. `sudo chmod -R 755 /opt/ml4seismic_tutorial`
This sets:
Owner: read, write, execute
Group: read, execute
Others: read, execute

