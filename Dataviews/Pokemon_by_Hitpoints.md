# Dataview List of Pokémon

This code displays a list of all Pokémon sorted by hit points. The dataview queries looks like:

````markdown
```dataview
LIST link(rows.file.link, rows.name)
FROM "Pokemon"
WHERE hitpoints != null AND hitpoints > 99
GROUP BY "**" + hitpoints + "**"
```

```dataview
LIST link(rows.file.link, rows.name)
FROM "Pokemon"
WHERE hitpoints != null AND hitpoints < 100
GROUP BY "**" + hitpoints + "**"
```
````

## List of Pokémon by Hit Points

```dataview
LIST link(rows.file.link, rows.name)
FROM "Pokemon"
WHERE hitpoints != null AND hitpoints < 100
GROUP BY "**" + hitpoints + "**"
```

```dataview
LIST link(rows.file.link, rows.name)
FROM "Pokemon"
WHERE hitpoints != null AND hitpoints > 99
GROUP BY "**" + hitpoints + "**"
```
