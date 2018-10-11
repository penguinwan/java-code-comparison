**I like B **

```
public class SecondLevel {
  Percentage getPercentage() {..}
}

public class ThirdLevel {
  Percentage getPercentage() {..}
}

public class SecondLevelDao {	
  Optional<SecondLevel> secondLevelDao.get() {..}
}

public class ThirdLevelDao {
  Optional<ThirdLevel> thirdLevelDao.get() {..}	
}

// --- A ---
public void process() {
  Optional firstLevel = ...
  Percentage p = firstLevel.orElseGet(() -> secondLevelDao
      .get(id1, id2)
      .map(SecondLevel::getPercentage)
      .orElseGet(() -> thirdLevelDao.get(id1)
        .map(ThirdLevel::getPercentage)
        .orElse(Percentage.ZERO)));	 
}

// --- B ---
public void process() {
  Percentage p = Percentage.ZERO;
  if(firstLevel.isPresent()) {
       p = firstLevel.get().getPercentage()
  } else {
      SecondLevel secondLevel = secondLevelDao.get(id1, id2)
      if(secondLevel.isPresent()) {
          p = secondLevel.get().getPercentage()
      } else {
          ThirdLevel thirdLevel = thirdLevelDao.get(id1)
          if(thirdLevel.isPresent()) {
              p = thirdLevel.get().getPercentage()
          }
      }
   }
}
```