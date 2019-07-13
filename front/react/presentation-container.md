
공식적으로 Container와 Component 로 구분된 것은 아니다.

리덕스를 사용할 때에는 특히 Container 와 Component 를 구분해주는게 좋습니다.
Container 는 앱의 상태를 관리하기 때문에 앱의 상태가 자주 바뀔수록 빈번하게 업데이트가 일어납니다.
그래서 필요없는 부분에 업데이트가 일어나지 않게 하려면 Container 와 Component 를 구분해주어야 합니다.
Container가 업데이트 되어도 그 아래 Component 와 상관이 없다면 업데이트가 일어나지 않습니다.
(Container 에 여러 Component 가 있을 경우)

안드로이드에서 activity 와 viewmodel 을 분리하는 느낌으로 보면 될 것같다.
명칭상으로 보면 presentation, container 의 구분이 맞을 듯 하다.

[presentation 과 container component](https://blueshw.github.io/2017/06/26/presentaional-component-container-component/)