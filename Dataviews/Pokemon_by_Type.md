# Dataview List of Pokémon

This code displays a list of all Pokémon grouped by type. The dataview query looks like:

````markdown
```dataview
LIST link(rows.file.link, rows.name)
FROM "Pokemon"
WHERE name != null AND type != null
GROUP BY "**" + type + "**"
```
````

## List of Pokémon by Type

```dataview
LIST link(rows.file.link, rows.name)
FROM "Pokemon"
WHERE name != null AND type != null
GROUP BY "**" + type + "**"
```
