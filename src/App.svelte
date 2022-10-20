<script>
  const data = [
    {
      game: 'Super Mario Odyssey',
      price: '49.99',
      releaseDate: '2017-10-27',
      rating: 97,
    },
    {
      game: 'The Legend of Zelda: Breath of the Wild',
      price: '59.99',
      releaseDate: '2017-03-03',
      rating: 97,
    },
    {
      game: 'Super Smash Bros. Ultimate',
      price: '59.99',
      releaseDate: '2018-12-07',
      rating: 93,
    },
    {
      game: 'Ori and the Will of the Wisps',
      price: '24.99',
      releaseDate: '2020-09-17',
      rating: 93,
    },
    {
      game: 'Animal Crossing: New Horizons',
      price: '49.99',
      releaseDate: '2020-03-20',
      rating: 90,
    },
  ];

  const STRING = 'string';
  const NUMBER = 'number';

  const fields = [
    {
      field: 'game',
      label: 'Game',
      type: STRING,
    },
    {
      field: 'price',
      label: 'Price',
      type: STRING,
    },
    {
      field: 'releaseDate',
      label: 'Release Date',
      type: STRING,
    },
    {
      field: 'rating',
      label: 'Rating',
      type: NUMBER,
    },
  ];

  const dateFormatter = new Intl.DateTimeFormat('en-US', {
    dateStyle: 'medium',
  });

  const comparators = {
    [STRING]: (a, b) => (
      a?.toLowerCase()?.localeCompare(b?.toLowerCase())
    ),
    [NUMBER]: (a, b) => a - b,
  };

  let sortField = 'game';
  let sortOrder = 1;
  let sortType = STRING;

  const sort = (field, type) => () => {
    sortField = field;
    sortType = type;
    sortOrder = sortField !== field
      ? 1
      : sortOrder * -1;
  };

  $: sortedData = data.sort((
    { [sortField]: a },
    { [sortField]: b }
  ) => comparators[sortType](a, b) * sortOrder);
</script>

<table>
  <caption>Sortable Table</caption>
  <thead>
    <tr>
      {#each fields as { field, label, order, type }}
        <th>
          <button
            class:asc={ sortOrder > 0 }
            class:desc={ sortOrder < 0 }
            class:selected={ sortField === field }
            on:click={ sort(field, type) }
          >
            { label }
          </button>
        </th>
      {/each}
    </tr>
  </thead>
  <tbody>
    {#each sortedData as { game, price, rating, releaseDate }}
      <tr>
        <td>{ game }</td>
        <td>${ price }</td>
        <td>{ dateFormatter.format(new Date(releaseDate)) }</td>
        <td>{ rating }</td>
      </tr>
    {/each}
  </tbody>
</table>

<style>
  table {
    font:normal 1em/1.5 arial,helvetica,sans-serif;
  }

  caption {
    padding: 0.4em 0.8em;
    font-weight: 600;
    font-size: 1.25em;
    background: #048;
    color: #FFF;
  }

  table {
    margin: 0 auto;
    border-collapse: collapse;
    background: #FFF;
    border: solid #048;
    border-width: 0 thin thin;
  }

  thead th {
    white-space: nowrap;
    background: #26A;
    color: #FFF;
  }

  th,
  td {
    padding: 0.25em 0.5em;
    text-align: center;
    border-bottom: thin solid #048;
  }

  thead th:first-child,
  tbody td:first-child {
    text-align: left;
  }

  tbody tr:nth-child(even) > * {
    background: #CDE;
  }

  button {
    padding:0 0 0 1.2em;
    font-weight:bold;
    font-family:inherit;
    font-size:1em;
    text-decoration:underline;
    border:none;
    background:transparent;
    color:inherit;
    cursor:pointer;
    padding: 0;
  }

  button:after {
    display: inline-block;
    width: 1.2em;
    text-align: right;
    content: "";
  }

  button.selected.asc:after {
    content: "\25BC";
  }

  button.selected.desc:after {
    content: "\25B2";
  }
</style>
