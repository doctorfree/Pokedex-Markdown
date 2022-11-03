# List Pokémon by Type and Abilities

If [Obsidian](https://obsidian.md) is being used to view these Pokémon Markdown documents in an Obsidian Vault and the [Dataview](https://github.com/blacksmithgu/obsidian-dataview) Obsidian Plugin is enabled then the following HTML forms can be used to query the Pokédex.

This is an example Obsidian dataview query of the Markdown Pokédex. Much more advanced queries can be performed using the Dataview and [CustomJS](https://github.com/saml-dev/obsidian-custom-js) Obsidian plugins.

## Select

### Pokémon Name
<input type="text" id="name" name="Enter a Pokémon Name"/>

### Pokémon Type
<select id="poketype" name="poketype">
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
<button type="button" onclick="myFunction()">Show Selected Type</button>

```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability"
FROM "Pokemon"
WHERE icontains(type, poketype.value)
SORT name ASC
```

### Pokémon Ability
<input type="text" id="ability" name="Enter a Pokémon Ability"/>

### Press OK to display
<input type="button" value="ok" />

## Type includes Grass and Ability includes Run Away
```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability"
FROM "Pokemon"
WHERE contains(type, "Grass") and contains(abilities, "Run Away")
```

## Type is Dark or Ability is Illusion
```dataview
TABLE WITHOUT ID
  file.link AS "Name",
  type AS "Type",
  abilities AS "Ability"
FROM "Pokemon"
WHERE type = "Dark" or abilities = "Illusion"
SORT name ASC
```

## See Also

- [National Pokédex](national_pokedex.md)
- [Pokédex Pokémon](Pokedex/pokemon.md)
- [Pokédex](pokedex.md)
- [Pokémon](pokemon.md)
- [README](README.md)
