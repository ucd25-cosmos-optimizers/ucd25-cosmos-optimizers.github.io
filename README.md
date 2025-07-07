# Demo for Group website

This website is built with [Hugo](https://gohugo.io/) and uses the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Guide

0. Create a group Github account (share this with other group members) and set user name as `ucd25-cosmos-<team name>`.
1. Click “**Use this template**” to create a new repository.
2. Repository title should be your **team name**, and make sure it is set to **public**.
3. Go to **Settings**->**Pages**, and under **Source**, select **Github Actions**.
4. Go to **Code** tab, and open `hugo.yaml` file.
5. Update the `baseurl` field to `https://ucd25-cosmos-<team name>.github.io/`.
6. Commit the changes.
7. Your website will be available at `https://ucd25-cosmos-<team name>.github.io/` after a few minutes.
8. Go to **Settings**->**Collaborators**, and add other team members.

## Assignments
Update the **Members** tab.

#### Instructions
- Open `content/members/_index.md`.
- Add each member's name and a short introduction.

(Optional)

- To include photos, upload image files to `content/members/` folder and reference the correct file names in `content/members/_index.md`.
- Update `README.md` file.
- To change the profile photo on the homepage, upload the image to the `static/` directory and update the `imageUrl` field in the `hugo.yaml` file.
