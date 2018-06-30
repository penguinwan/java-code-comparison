**I like B, no argue**

```
// --- A ---
private static TransactionPosition merge(TransactionPosition fst, TransactionPosition snd) {
    Function<Function<TransactionPosition, BigDecimal>, BigDecimal> sum = sumProperty(fst, snd);
    TransactionPosition res = new TransactionPosition();
    res.setId(fst.getId());
    res.setLevel(fst.getLevel());
    res.setAmount(sum.apply(TransactionPosition::getCommission));
    res.setOverride(sum.apply(TransactionPosition::getOverride));
    return res;
}
private static <T> Function<Function<T, BigDecimal>, BigDecimal> sumProperty(T i1, T i2) {
    return getter -> {
        BigDecimal a = getter.apply(i1);
        BigDecimal b = getter.apply(i2);
        if (a == null) {
            a = BigDecimal.ZERO;
        }
        if (b == null) {
            b = BigDecimal.ZERO;
        }
        return a.add(b);
    };
}

// --- B ---
private static TransactionPosition merge(TransactionPosition fst, TransactionPosition snd) {
    TransactionPosition res = new TransactionPosition();
    res.setId(fst.getId());
    res.setLevel(fst.getLevel());
    res.setCommission(totalOf(fst.getCommission(), snd.getCommission()));
    res.setOverride(totalOf(fst.getOverride(), snd.getOverride()));
    return res;
}
private static BigDecimal totalOf(BigDecimal fst, BigDecimal snd) {
    return valueOf(fst).add(valueOf(snd));
}
private static valueOf(BigDecimal value) {
    if(value == null) {
        return BigDecimal.ZERO;
    } else {
        return value;
    }
}

// --- C ---
private static TransactionPosition merge(TransactionPosition fst, TransactionPosition snd) {
    TransactionPosition res = new TransactionPosition();
    res.setId(fst.getId());
    res.setLevel(fst.getLevel());
    res.setCommission(ADDITION.apply(fst.getCommission()).apply(snd.getCommission()));
    res.setOverride(ADDITION.apply(fst.getOverride()).apply(snd.getOverride()));
    return res;
};
private static Function<BigDecimal, Function<BigDecimal, BigDecimal>> ADDITION = a -> b -> {
    return VALUE_OF.apply(c).add(VALUE_OF.apply(d));
};
private static Function<BigDecimal, BigDecimal> VALUE_OF = value -> {
    if(value == null) {
        return BigDecimal.ZERO;
    } else {
        return value;
    }
}
```