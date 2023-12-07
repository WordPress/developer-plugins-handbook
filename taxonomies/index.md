## Taxonomies

A **Taxonomy** is a fancy word for the classification/grouping of things. Taxonomies can be hierarchical (with parents/children) or flat.

WordPress stores the Taxonomies in the `term_taxonomy` database table allowing developers to register Custom Taxonomies along the ones that already exist.

Taxonomies have **Terms** which serve as the topic by which you classify/group things. They are stored inside the `terms` table.

For example: a Taxonomy named "Art" can have multiple Terms, such as "Modern" and "18th Century".

This chapter will show you how to register Custom Taxonomies, how to retrieve their content from the database, and how to render them to the public.

[info]WordPress 3.4 and earlier had a Taxonomy named "Links" which was deprecated in WordPress 3.5.[/info]