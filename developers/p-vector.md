<!-- TITLE: p-vector -->
<!-- SUBTITLE: Information about the deb repo manager p-vector -->

# Introduction
P-vector manages AOSC OS's deb repository, generates index files and analyzes problems in packaging.

# Usage
`p-vector config.yaml (scan|release [--force]|sync|analyze [full]|reset [pv|sync])`

* `scan`: scan the directories containing packages and record them in the database.
* `release`: generate the `Packages`, `InRelease` and `Contents` files for apt.
* `sync`: download SQLite databases from packages.aosc.io and load them into the PostgreSQL database.
* `analyze`: analyze problems in packaging.
* `reset`: drop tables.

# Database
p-vector uses a PostgreSQL database. We recommend using the latest version, even though 9.6 is still usable.

## Tables

### p-vector native tables
* pv_repos
* pv_packages
* pv_package_duplicate
* pv_package_dependencies
* pv_package_sodep
* pv_package_files
* pv_package_issues
* pv_issues_stats
* pv_dbsync

### tables from packages site
#### abbs.db
* trees
* tree_branches
* packages
* package_dependencies
* package_duplicate
* package_spec
* package_versions
* dpkg_repo_stats

#### \*-marks.db
* repo_branches
* repo_committers
* repo_marks
* repo_package_basherr
* repo_package_rel

#### piss.db
* package_upstream
* upstream_status
* anitya_link
* anitya_projects

## Views
These views come from abbs.db of the packages site.

* v_package_upstream
* v_packages

## Materialized Views
These views show more information about packages and their relationships, but are expensive to compute.

* v_dpkg_dependencies
* v_file_conflict
* v_package_issues
* v_packages_new
* v_so_breaks
* v_sodep_links
