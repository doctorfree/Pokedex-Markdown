# Pokédex Queries

The Pokédex can be queried using the [Obsidian Dataview Plugin](https://blacksmithgu.github.io/obsidian-dataview).

## Local Obsidian dataview plugin queries

If [Obsidian](https://obsidian.md) is being used to view these Pokémon Markdown documents in an Obsidian Vault and the [Dataview](https://github.com/blacksmithgu/obsidian-dataview) Obsidian Plugin is enabled then it is possible to query the Pokédex using Dataview.

The Pokémon markdown has been curated to include metadata describing many Pokémon attributes. These include:

```yaml
name: Pokémon Name
type: Pokémon Types
abilities: Pokémon Abilities
hitpoints: Hit Points
attack: Attack Strength
defense: Defense Strength
specialattack: Special Attack Strength
specialdefense: Special Defense Strength
speed: Speed
total: Total Points
```

Dataview queries can combine these attributes in a variety of ways to retrieve Pokémon matching the query terms. For example, to retrieve all Pokémon with Hit Points greater than 200, add the following Dataview code block to a markdown document in this vault:

````markdown
```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability",
  hitpoints AS "Hit Points"
FROM "Pokemon"
WHERE hitpoints > 200
SORT name ASC
```
````

The above Dataview code block produces the following output:

```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability",
  hitpoints AS "Hit Points"
FROM "Pokemon"
WHERE hitpoints > 200
SORT name ASC
```

Or, if you want to find all Pokémon in the Pokédex of type "Dragon" with Hit Points greater than 200, then combine queries:

````markdown
```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability",
  hitpoints AS "Hit Points"
FROM "Pokemon"
WHERE hitpoints > 200 and contains(type, "Dragon")
SORT name ASC
```
````

The above Dataview code block produces the following output:

```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability",
  hitpoints AS "Hit Points"
FROM "Pokemon"
WHERE hitpoints > 200 and contains(type, "Dragon")
SORT name ASC
```

## Query the Pokédex on a website

If the Pokédex has been deployed on a website then something like the following could be used to allow website visitors to query the Pokédex:

```html
<select id="poketype" name="poketype">
  <option value="none" selected disabled hidden>Select a Type</option>
  <option value="bug">Bug</option>
  <option value="dark">Dark</option>
  <option value="dragon">Dragon</option>
  <option value="electric">Electric</option>
  <option value="fairy">Fairy</option>
  <option value="fighting">Fighting</option>
  <option value="fire">Fire</option>
  <option value="flying">Flying</option>
  <option value="ghost">Ghost</option>
  <option value="grass">Grass</option>
  <option value="ground">Ground</option>
  <option value="ice">Ice</option>
  <option value="normal">Normal</option>
  <option value="poison">Poison</option>
  <option value="psychic">Psychic</option>
  <option value="rock">Rock</option>
  <option value="shadow">Shadow</option>
  <option value="steel">Steel</option>
  <option value="water">Water</option>
  <option value="unknown">Unknown</option>
</select>
<button type="button" onclick="showType()">Show Selected Type</button>
```

Which looks something like:

<select id="poketype" name="poketype">
  <option value="none" selected disabled hidden>Select a Type</option>
  <option value="bug">Bug</option>
  <option value="dark">Dark</option>
  <option value="dragon">Dragon</option>
  <option value="electric">Electric</option>
  <option value="fairy">Fairy</option>
  <option value="fighting">Fighting</option>
  <option value="fire">Fire</option>
  <option value="flying">Flying</option>
  <option value="ghost">Ghost</option>
  <option value="grass">Grass</option>
  <option value="ground">Ground</option>
  <option value="ice">Ice</option>
  <option value="normal">Normal</option>
  <option value="poison">Poison</option>
  <option value="psychic">Psychic</option>
  <option value="rock">Rock</option>
  <option value="shadow">Shadow</option>
  <option value="steel">Steel</option>
  <option value="water">Water</option>
  <option value="unknown">Unknown</option>
</select>
<button type="button" onclick="showType()">Show Selected Type</button>

```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability"
FROM "Pokemon"
WHERE type = "Dark" and abilities = "Illusion"
SORT name ASC
```

On the backend Dataview queries could be performed by the `showType` function using Dataview code blocks:

### Grass type with "Water Absorb" ability OR Abilities contain Gluttony and Liquid Ooze

````markdown
```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability"
FROM "Pokemon"
WHERE (type = "Grass" and 
       contains(abilities, "Water Absorb")) or
      (contains(abilities, "Gluttony") and
       contains(abilities, "Liquid Ooze"))
```
````

Which produces:

```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability"
FROM "Pokemon"
WHERE (type = "Grass" and 
       contains(abilities, "Water Absorb")) or
      (contains(abilities, "Gluttony") and
       contains(abilities, "Liquid Ooze"))
```

### Type is Dark and Ability is Illusion

````markdown
```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability"
FROM "Pokemon"
WHERE type = "Dark" and abilities = "Illusion"
SORT name ASC
```
````

Would produce:

```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability"
FROM "Pokemon"
WHERE type = "Dark" and abilities = "Illusion"
SORT name ASC
```

## See Also

- [National Pokédex](national_pokedex.md)
- [Pokédex Pokémon](Pokedex/pokemon.md)
- [Pokédex](pokedex.md)
- [Pokémon](pokemon.md)
- [List of Pokémon](list_of_pokemon.md)
- [Pokémon Generations](generations.md)
- [README](README.md)
