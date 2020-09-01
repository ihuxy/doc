## components

### Spinner

```
const Spinner=({global})=>{
  const className=global?`${styles.spinner} ${styles.global}`:styles.spinner;
  return <div className={className}>
    <figure className={styles.spinning} />
  </div>;
};

```


## Anico

[anico](../styles/anico.md)

```
const Anico=({type})=>{
  const className=type?`${styles.hline} ${styles[type]}`:styles.hline;
  return <span className={styles.anico}>
    <span className={className} />
  </span>;
};

```
