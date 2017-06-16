# diagram
ui(f)>presenter(i)>usecases(b)>entities(d)
f:Frameworks and Drivers
i:Interface Adapters
b:Business rules
d:Domain logic

# Architectural approach
Presentation(Model View) - Domain(Regular Java) - Data(Repository)

# Architectural reactive approach
View > Presenter > Domain Use Case > Repository > Disk/Cloud
   subscriber         observable     observable
data direction <---------------------------------

# 궁금한거
mvvm 과 cleanarchitecture
