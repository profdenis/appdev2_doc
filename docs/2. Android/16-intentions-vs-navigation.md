# 16. Intents vs. Navigation

## `Intent` vs. `Navigation`

### Navigation Compose

#### Advantages

- **Type safety**: Compile-time checks, reducing potential errors
- **Native integration** with Jetpack Compose and modern Android architecture
- **More readable and maintainable code**, less repetitive code
- **Declarative approach** that simplifies navigation

#### Disadvantages

- Limited to basic data types for navigation arguments
- Requires adding an additional dependency

### Intent Navigation

#### Advantages

- **Flexibility**: Usable in any Android project
- **Familiarity**: Extensive documentation and community support
- **Ability to pass Parcelable objects** easily
- **Easy integration** with other Android components

#### Disadvantages

- **No compile-time checking**: Risk of runtime errors
- **More verbose**: Requires more code for navigation management
- **Manual management** of lifecycle and state

### Recommendation

For a new application using Jetpack Compose:

- Prefer **Navigation Compose** for internal application navigation
- Use **Intents** only for:
    - Navigation to other applications
    - Cases requiring passing complex objects
    - Integration with traditional Android components

This hybrid approach allows benefiting from Navigation Compose advantages while maintaining Intent flexibility when
needed.

Citations:

- https://stackoverflow.com/questions/65088035/how-to-navigate-from-a-composable-to-an-activity-in-jetpack-compose
- https://guides.peruzal.com/v1/android-guides/navigation/intents/
- https://www.geeksforgeeks.org/start-a-new-activity-using-intent-in-android-using-jetpack-compose/
- https://developer.android.com/develop/ui/compose/navigation
- https://developer.android.com/develop/ui/compose/navigation?hl=fr
- https://betterprogramming.pub/intent-based-compose-navigation-1087634b984a
- https://blog.kotlin-academy.com/mastery-navigation-in-jetpack-compose-db00b0a0ef75?gi=007e50484ede
- https://rommansabbir.com/typesafe-navigation-or-traditional-intent-passing
