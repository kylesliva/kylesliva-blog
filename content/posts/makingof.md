---
title: 'Building this site'
date: "2024-03-22T13:01:01-04:00"
draft: false
---

# Introduction

I wanted to build my own personal website, using tools from my day job as a fun exercise to stay current on my technical skills. The idea of a flat file CMS appealed to me greatly: no complex tapestry of backend plugins or server administration to worry about. Plus: all served content is raw Markdown at its core! This allows me to simply deploy my site to an Amazon S3 bucket, and then use CloudFront to serve it globally.
# Special thanks

I based this process off of [Logan Marchione's wonderful guide](https://loganmarchione.com/2023/11/deploying-hugo-with-cloudfront-and-s3-for-real-this-time/) on how he set his site up (you may notice that my theme is the exact same!) I got a great starting point from there, and was able to customize it a bit to fit my needs. 

Please refer to Logan's guide for the complete overview of how my site is set up, as I have customized it for my own use case. 
# Repos

1. https://github.com/kylesliva/kylesliva-tf-infra
	1. Raw Terraform infrastructure for my DNS setup, as I migrated it to Route 53 in the process of setting this site up.
2. https://github.com/loganmarchione/terraform-aws-static-site
	1. Back-end Terraform infrastructure for CloudFront, S3, ACM, and other ancillary AWS services. 
	2. Triggered as a module from the content repo below.
3. https://github.com/kylesliva/kylesliva-blog
	1. Source for my blog's content, TF module invocation, and CloudFront function.
	2. Large files like content and public site are omitted for brevity. 

# Lessons learned
* Hugo is very complex! Here are a few of the problems I had to solve before I could consider my site "generally available" :)
	* Search indexing had to be enabled by setting JSON output in `hugo.yaml`, in addition to setting up a search page above in that file. 
* Images seem to only be callable from the `static/*` folder. This may be adjustable but I need to learn more about hugo works first.
* You can use YAML for front matter/config files, but you must set `archetypes/default.md` to force Hugo to respect that. On top of that, Hugo's documentation prefers TOML so sometimes a bit of trial and error is required to get everything playing nice.
* I had issues getting the site to take a user back to the home page when they clicked my name in the top-right corner, but [setting up a customized CloudFront function](https://loganmarchione.com/2023/11/deploying-hugo-with-cloudfront-and-s3-for-real-this-time/#functions) along with the `baseURL` argument in `hugo.yaml` solved the problem for me. 

# Basic workflow
1. Write Markdown content with images specified
	1. I use [Obsidian](https://obsidian.md/) as a kind of digital notebook, so a Markdown-based CMS is a natural fit. I can just grab the raw Markdown source and push it to my local site source, which then gets rendered into a page like the one  you're reading now!
2. Save images to `static/`, making sure names match the Markdown source code.
3. Create new content in Hugo locally:
```
$ hugo new content posts/2024/makingof.md
```
4. Grab raw Markdown source from Step 1. Populate file with the latter, taking care not to overwrite the front matter (the YAML syntax at the top of the file, delimited with `---`)
5. Navigate to the project source. Test on a local server with:
```
$ hugo server --buildDrafts 
```
6. Verify the output looks good, adjust as needed. When happy with results, set the `draft:` argument in the content's front matter to `false`.
7. Deploy to S3 with:
```
$ hugo; hugo deploy --target site-bucket --logLevel INFO
```
 8. Navigate to public site (in this case https://kylesliva.me)
 9. Verify site matches intent. You're all done! 

# What to do next?
1. Set up GitHub Actions deploys to automate step 4 onwards from my workflow. I am looking at using [this](https://loganmarchione.com/2023/11/deploying-hugo-with-cloudfront-and-s3-for-real-this-time/#github-actions) as a model, but I'd like to sit down and really spend time customizing it to fit how I do my work.
2. Writing automation for steps 1-3 of my workflow
	1. Get from Obsidian to my repo at a minimum of effort.
