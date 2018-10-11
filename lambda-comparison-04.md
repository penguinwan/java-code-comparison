**I like B **

```
public class Level {
  Percentage getPercentage() {..}
}

public class SecondLevelDao {	
  Optional<Level> secondLevelDao.get() {..}
}

public class ThirdLevelDao {
  Optional<Level> thirdLevelDao.get() {..}	
}

// --- A ---
public void process() {
  Optional firstLevel = ...
  Percentage p = firstLevel.orElseGet(() -> secondLevelDao
      .get(id1, id2)
      .map(Level::getPercentage)
      .orElseGet(() -> thirdLevelDao.get(id1)
        .map(Level::getPercentage)
        .orElse(Percentage.ZERO)));	 
}

// --- B ---
public void process() {
  Percentage p = Percentage.ZERO;
  if(firstLevel.isPresent()) {
       p = firstLevel.get().getPercentage()
  } else {
      Level secondLevel = secondLevelDao.get(id1, id2)
      if(secondLevel.isPresent()) {
          p = secondLevel.get().getPercentage()
      } else {
          Level thirdLevel = thirdLevelDao.get(id1)
          if(thirdLevel.isPresent()) {
              p = thirdLevel.get().getPercentage()
          }
      }
   }
}
```