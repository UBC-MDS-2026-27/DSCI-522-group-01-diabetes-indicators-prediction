# Feedback and Contribution

Input, feedback, bug reports, and contributions to our analysis can be made via the [Diabetes Indicator Prediction GitHub Repository](https://github.com/UBC-MDS-2026-27/DSCI-522-group-01-diabetes-indicators-prediction)

## How to contribute

### Setting up your environment

Development and running of the analysis is all done in the same Docker container, this ensures everyone uses the same software and package versions. Follow the [Usage section of the README](README.md#usager)(install Docker, clone the repository, make an ssh key, and download the image). Run all commands below from the root of this repository.

### Working in the container

We edit and run the code inside the container, by connecting Positron to it over ssh.
Your project folder is shared with the container at `/workspace`,
so a file you change there changes on your computer too.

<details>
<summary><strong>Once, before your first connection</strong></summary>

1. Install [Positron](https://positron.posit.co/download.html) on your computer. Positron has a built-in Remote SSH feature which will connect to the container, therefore you do not need an extension.

2. Ensure you have followed the steps in the [Usage section of the README](README.md#usage):
   you have an ssh key at `~/.ssh/id_ed25519.pub` and you ran `docker compose pull`

3. Check that Git knows who you are on your computer:

   ``` bash
   git config --global user.email
   ```

   It should print the email of your GitHub account.
   If it prints nothing, set your name and email:

   ``` bash
   git config --global user.name "Your Name"
   git config --global user.email "you@example.com"
   ```

   `docker-compose.yml` shares your `~/.gitconfig` with the container,
   so the commits you make there count as yours.

</details>

<details>
<summary><strong>Each time you work on the project</strong></summary>

1. Within the terminal of your project folder, start the container in the background: 

   ``` bash
   docker compose up -d
   ```

   `docker compose ps` should list `analysis-env` as running.

2. In Positron, open the Command Palette
   (`Cmd+Shift+P` on Mac, `Ctrl+Shift+P` on Windows).

3. Type and run **Remote SSH: Show Remote Menu**, then pick **Connect to Host**.

4. Type `root@localhost:2222` and press Enter. A new Positron window opens.
   * If it asks for the platform of the remote host, pick **Linux**.
   * If it asks whether you trust the host (a "fingerprint"), click **Continue**.
   * The first time, Positron installs its server inside the container. This make take a few minutes 

5. **Open the project folder.**
   The bottom left corner now shows `SSH: localhost:2222`,
   but the Explorer says **No folder opened** and the terminal starts in `/root`,
   which has no project files. That is normal.
   * Click **Open Folder** in the Explorer.
   * Delete what is in the path box at the top of the window, type `/workspace`, and click "ok" (you may need to click "ok" a few times before this works).
   * The window reloads, and the Explorer shows **WORKSPACE [SSH: LOCALHOST:2222]** with the project files.

6. **Pick the Python interpreter** (check this every time).
   * Click the interpreter button at the top right of the window.
     The **Running Interpreter Sessions** menu only lists sessions that are already running
     (such as `/usr/bin/python3`, the container's system Python: not this one).
   * Click **New Console Session...** and pick
     **Python 3.x (uv: DSCI-522-group-01-diabetes-indicators-prediction)**,
     with the path `/opt/venv/bin/python`. This is the environment built from `uv.lock`.
   * If it isn't in the list, run **Python: Select Interpreter** from the Command Palette,
     choose **Enter interpreter path...** and type `/opt/venv/bin/python`.
   * Don't pick `/workspace/.venv`: that is an environment from your own computer,
     and it doesn't run in the container.
   * To check, run `import sys; print(sys.executable)` in the Console.
     It should print `/opt/venv/bin/python`.

7. Work as usual. The Terminal, the Console and the **Render** button all run inside the container.

8. When you are done, close the Positron window,
   then stop and remove the container in a terminal on your computer:

   ``` bash
   docker compose down
   ```

<details>

<details>
<summary><strong>If it doesn't connect</strong></summary>

* **`~/.ssh/id_ed25519.pub` is a folder:** Docker made it because the key didn't exist yet.
  Run `docker compose down`, delete that folder, make the key
  (see the [README](README.md#usage)), and start again.
* **`~/.gitconfig` is a folder, or Git in the container doesn't know your name:**
  the same thing happened with your Git settings.
  Run `docker compose down`, delete that folder, set your name and email (see above), and start again.
* **It asks for a password:** the container can't see your key.
  Check that your key is named `id_ed25519.pub`, or change the key path in `docker-compose.yml`.
* **"Remote host identification has changed":** you are using a new image.
  Run `ssh-keygen -R "[localhost]:2222"`, then connect again.
* **GitHub asks you to sign in when you push from the container:**
  sign in once in the browser window Positron opens. Positron remembers it.
* **Linux laptops:** ssh refuses the key because the file belongs to you, not `root`.
  Remove the `#` from the `StrictModes no` line in the `Dockerfile` and push,
  so the workflow publishes a fixed image.
* **Port 2222 is already in use:** change the first `2222` in `docker-compose.yml`
  to another number (e.g. `2223`), and connect to that port instead.

More detail: [Working in a container over ssh with Positron](https://ubc-dsci.github.io/reproducible-and-trustworthy-workflows-for-data-science/lectures/containers-ssh-positron.html).

</details>

To re-run the analysis and render the report without opening the container:

``` bash
docker compose run --rm analysis-env uv run quarto render
```

### Adding or updating a package

On a new branch, add the package with `uv add <package>==<version>`
(this pins the version in `pyproject.toml` and updates `uv.lock`),
then commit the changed `pyproject.toml` and `uv.lock` together and push them.
GitHub Actions then builds and publishes a new image on Docker Hub,
and commits the new `sha-` tag to the `image:` line of `docker-compose.yml`
on your branch.
When the workflow finishes,
get that commit and download the new image:

``` bash
git pull
docker compose pull
```

Then merge the branch into `main` in a pull request.

### Publishing the report

The report on GitHub Pages is a copy of the rendered report in `docs/index.html`.
Each time you re-render the report, copy it there again, commit and push:

``` bash
cp reports/breast_cancer_predictor_report.html docs/index.html
```

### Creating a branch

Create a new branch to contain a new contribution, contributions should be pushed to the new branch. It is important to always create a new branch instead of directly making changes to the main branch. 

```bash
git switch -c <your-branch-name>
```

Branch naming convention should follow this format

```bash
<type>/<description>
```

Example branch name

```bash
docs/updating-contribution-guidelines
```
### Creating a pull request

After your contributions are made, they can be committed to the corresponding branch by running

```bash
git add <modified-file>
git commit -m "descriptive message about the contribution made"
git push origin <your-branch-name>
```

After pushing you will need to submit a pull request on GitHub asking to merge the new contributing branch to the main branch. Please include details/explaination of the contribution within the pull request and assign all current team members as reviewers of the request. Feedback to the pull request will be given in the pull request discussion. 

## Code of conduct

This project's code of conduct can be found [here](https://github.com/UBC-MDS-2026-27/DSCI-522-group-01-diabetes-indicators-prediction/blob/main/CODE_OF_CONDUCT.md). By contributing to this project you agree to abide by these terms.

## AI use policy

The definition of "AI" under this policy includes generative AI tools such as large language models (LLMs) that have the capabilities to generate, edit, and review code. 

### Responsibility

Any code submitted by you that is written manually or generated by AI is your responsibility. You must be able to walk through and explain any code you submit, it is unacceptable to submit code that you cannot explain upon review. 

### Disclosure

It is also your responsibility to maintain transparency and disclose AI usage. AI use or AI assistance during any stage of your contribution must be disclosed, including which tools, how they were used, and which portion of the code was generated or assisted. 

## Attributions

The above contribution guidelines were adapted from [Altair CONTRIBUTING.md](https://github.com/vega/altair/blob/master/CONTRIBUTING.md?plain=1) and [DSCI 522 Project Template CONTRIBUTING.md](https://github.com/UBC-MDS/DSCI_522_project-template-breast-cancer-predictor/blob/main/CONTRIBUTING.md). Our AI use policy guidelines were adapted from [SciPy](https://scipy.github.io/devdocs/dev/conduct/ai_policy.html).