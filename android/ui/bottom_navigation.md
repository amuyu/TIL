# BottomNavigationView & navController
menu 파일에서 id 로 fragment 이름을 사용한다.

```kotlin
val navView: BottomNavigationView = findViewById(R.id.nav_view)
//        navView.setOnNavigationItemSelectedListener(onNavigationItemSelectedListener)
navView.setOnNavigationItemSelectedListener { item ->
    NavigationUI.onNavDestinationSelected(item, findNavController(R.id.navigation_fragment))
}

val navController = this.findNavController(R.id.navigation_fragment)
NavigationUI.setupWithNavController(navView, navController)
```