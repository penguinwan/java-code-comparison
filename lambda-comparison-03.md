**I like B**

```
// --- A ---
public enum State {
    WAITING(0), ASSIGNED(1), TRANSACTED(2), CANCELLED(3);

    private int id;
    State(int id) {
        this.id = id;
    }
    public int getId() {
        return id;
    }
    public static State get(int id) {
        return Arrays.stream(values())
                .filter(x -> x.getId() == id)
                .findAny()
                .orElseThrow(() -> new IllegalArgumentException("Unsupported transaction state with id '" + id + "'"));
    }
}

// --- B ---
public enum State {
    WAITING(0), ASSIGNED(1), TRANSACTED(2), CANCELLED(3);

    private int id;
    State(int id) {
        this.id = id;
    }
    public int getId() {
        return id;
    }
    public static State get(int id) {
        if(id == 0) {
            return WAITING;
        } else if (id == 1) {
            return ASSIGNED;
        } else if (id == 2) {
            return TRANSACTED;
        } else if (id == 3) {
            return CANCELLED;
        } else {
            throw new IllegalArgumentException("Unsupported transaction state with id '" + id + "'");
        }
    }
}
```