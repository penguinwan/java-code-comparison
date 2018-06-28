**I like B**

```
// --- A ---
@Test
public void test() { 
	Assert.assertArrayEquals("Expected: " + Arrays.toString(expected) + " Result: " + Arrays.toString(result), expected, result);
}

// --- B ---
@Test
public void test() {
	ASSERTION.resultOf(result).isExpected(expected).otherwiseShow(assertionMessage(expected, result));
}

private Assertion ASSERTION = r -> e -> m -> Assert.assertArrayEquals(assertionMessage(e, r), e, r);

@FunctionalInterface
private interface Assertion {
    Expected resultOf(int[] value);
}
@FunctionalInterface
private interface Expected {
    Otherwise isExpected(int[] value);
}
@FunctionalInterface
private interface Otherwise {
    void otherwiseShow(String message);
}
```