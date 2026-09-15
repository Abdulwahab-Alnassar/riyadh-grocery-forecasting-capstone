# Submission and GitHub guide

The forecasting work is complete in one executed notebook. These remaining steps
depend on the trainee's actual cohort details and GitHub account.

## Fill personal metadata

- Confirm the author spelling in the README and notebook header.
- Enter the actual cohort start and end dates in the README, notebook header,
  and the `config` dictionary in the export cell. No cohort dates were invented.
- If this is a team submission, add the actual team members and their contributions.

## Publish with a real Colab link

Create a public or private repository in your own GitHub account. Suggested name:
`riyadh-grocery-forecasting-capstone`.

Suggested repository description:

> Backtested 28-day forecasts of synthetic Riyadh grocery demand using Holt-Winters and LightGBM, with chronological validation and calibrated prediction intervals. SDAIA Academy capstone.

Upload the notebook, README, `.gitignore`, `requirements-tested.txt`, `data/`, and
`docs/`. The single notebook is self-contained; the CSV is an inspectable copy of
the same embedded snapshot. Generated runtime artifacts belong outside future
routine commits; the captured notebook outputs are intentionally retained.

Replace the README badge destination with your actual address:

```text
https://colab.research.google.com/github/YOUR_USERNAME/YOUR_REPOSITORY/blob/main/Riyadh_Grocery_Forecasting_Capstone.ipynb
```

For private repositories, access in Colab depends on the viewer's GitHub access.
Test the actual link after publishing. Do not submit the generic picker link as
if it were a direct link to a published notebook.

## Preserve the actual local development commits

The archive includes `capstone-history.bundle`, which captures real successive
build stages using the agent identity `Codex <codex@local>`. It does not fabricate
earlier work dates or impersonate the trainee. If you want to preserve this history,
run the following from the extracted directory:

```bash
git clone capstone-history.bundle riyadh-grocery-forecasting-capstone
cd riyadh-grocery-forecasting-capstone
git remote remove origin
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
```

Replace the placeholders with your real repository. Configure your own Git identity,
then make a meaningful commit adding cohort details, any team information, and the
real Colab badge. Push to the repository you created:

```bash
git add README.md Riyadh_Grocery_Forecasting_Capstone.ipynb
git commit -m "Add cohort details and repository Colab link"
git push -u origin main
```

The bundle is optional for opening Colab. Do not upload `.git/`, credentials, or
the history bundle into the repository. Keep improving the project through genuine
commits; the course expects an active, maintained repository, not fabricated history.

## Final check

- Fresh Colab **Run all** completes without manual data upload.
- The displayed metrics match your current run; interpret new numbers if you change settings.
- The seven analysis sections and comparison prose remain inside the one notebook.
- Both interval coverage and width are visible, together with the nominal level.
- README, technical documentation, cohort dates, and SDAIA Academy link are present.
- GitHub repository and Colab links point to your actual project and can be opened
  by the instructor with the access you intended.
