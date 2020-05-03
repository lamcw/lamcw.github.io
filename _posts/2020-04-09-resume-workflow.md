---
layout: post
title: "My Resume Workflow"
categories:
- DevOps
- LaTeX
mathjax: true
---

I have been writing and compiling my resume on [Overleaf](https://www.overleaf.com), and hosting [the project](https://github.com/lamcw/resume) on GitHub. Just to give you a bit of its background, Overleaf is something like Google Docs for \\(\LaTeX\\). It provides a \\(\LaTeX\\) environment in your browser, and you can collaborate on the same document with other users.

Overleaf is good for creating \\(\LaTeX\\) documents, but I want to have my resume readily available on the Internet, and ideally, whenever I make changes to the \\(\TeX\\) file, a compiled version of the document will be uploaded to a file hosting site. Now Overleaf and even other \\(\TeX\\) distributions are not all-purpose -- the only things you can do in the Overleaf are: export the project as PDF, or sync the project with Dropbox, Git or GitHub.

One way I could think of, is to trigger a build whenever I push changes to/tag a commit in the GitHub repo, and automagically upload the artifacts to GitHub. Free file hosting, great! To do so, let's create a GitHub Actions workflow in a repo that contains a `.tex` file, which will:

1. Compiles the \\(\TeX\\) source file into PDF
2. Create a release on GitHub
3. Upload the PDF as a part of the release

{% raw %}
```yaml
name: CI

on:
  push:
    tags:
      - 'v*'

jobs:
  build_latex:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@master
      - name: Compile LaTex Resume
        uses: xu-cheng/latex-action@master
        with:
          root_file: resume.tex
      - name: Create Release
        id: create_release
        uses: actions/create-release@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
          draft: false
          prerelease: false
      - name: Upload Release Asset
        id: upload-release-asset 
        uses: actions/upload-release-asset@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./resume.pdf
          asset_name: resume.pdf
          asset_content_type: application/pdf
```
{% endraw %}

I added a condition:
```yaml
on:
  push:
    tags:
      - 'v*'
```
so that it only creates a release when I tag a commit. Voilà! The PDF should then be available at `https://github.com/user/repo_name/releases/latest/download/resume.pdf`. I have made my resume repo a "template" so that you can use the same workflow (or you can copy & paste the file :wink:). This method does not only work for Overleaf, it works for any \\(\TeX\\) distributions!

---

Thanks for reading my first blog post! It's been quite some time since I started making this site[^1]. Really enjoyed building this site from scratch, and hopefully I will keep updating the blog.

[^1]: [First commit](https://github.com/lamcw/lamcw.github.io/commit/d85f1bc2b17f876931ba36476142b633a49123d0) was on Jun 26, 2018, lol
