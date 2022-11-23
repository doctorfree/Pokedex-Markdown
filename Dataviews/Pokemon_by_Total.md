# Dataview List of Pokémon

This code displays a list of all Pokémon sorted by total points. The dataview queries looks like:

````markdown
```dataview
LIST link(rows.file.link, rows.name)
FROM "Pokemon"
WHERE total != null
GROUP BY "**" + total + "**"
```
````

## List of Pokémon by Hit Points

```dataview
LIST link(rows.file.link, rows.name)
FROM "Pokemon"
WHERE total != null
GROUP BY "**" + total + "**"
```
