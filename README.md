# OOSQ

Object-oriented sequel(-style) queries

## Query functions

### deleteFrom

Removes an item from the dataset where condition matches.

```ts
const { result } = deleteFrom<Item>(data).where((item) => item.id === 2);
```

**Note**: Unlike SQL, there is no default behavior for `deleteFrom`. If you want to remove all items from the dataset, pass `Boolean` or `() => true` to `where()`.

#### Methods

- `where`

### insertInto

Adds an item to the dataset.

```ts
const { result } = insertInto<Item>(data).set({ id: 3, name: "cashews" });
```

#### Methods

- `set`

### selectFrom

Returns items that match the provided conditions

```ts
const { result } = selectFrom<Item>(data)
  .where(({ value }) => value > 4)
  .orderBy("id", "ASC")
  .columns(["name", "value"]);
```

**Notes**: Unlike SQL, the order of the statements matters.

#### Methods

- `columns`
- `orderBy`
- `where`

### update

Updates all of the items which match the provided condition

```ts
const { result } = update<Item>(data)
  .where((item) => item.id === 3)
  .set((item) => ({ ...item, name: "hazelnuts" }));
```

#### Methods

- `set`
- `where`

## Schema

To reduce the boilerplate for creating queries, you can use the `Schema` function:

```ts
const q = Schema<Item>();

q.deleteFrom(data).where(/* ... */);
q.insertInto(data).values(/* ... */);
q.selectFrom(data).columns(/* ... */);
q.update(data).where(/* ... */);
```

The functions returned all received the initial item type.

If you are working with an external data source like `localStorage`, you can go one step further and pass a resolver to `Schema`:

```ts
const q = Schema<Item>(() =>
  JSON.stringify(localStorage.getItem("example") as Item[])
);

q.deleteFrom().where(/* ... */);
q.insertInto().values(/* ... */);
q.selectFrom().columns(/* ... */);
q.update().where(/* ... */);
```

## License

[ISC](./LICENSE) © 2021 [Sean McPherson](https://seanmcp.com)
