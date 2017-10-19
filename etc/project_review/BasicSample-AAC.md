# android component architecture - BasicSample
MVVM
## ProduceListFragment
Databinding, LifecycleFragment, AndroidViewModel 사용
### data 갱신
viewmodel 에 mObservableProducts 를 observe 해서 변화가 발생하면 adapter list 갱신
### list click 이벤트
callback을 adpater에 넘겨주고 binding 을 이용해 xml 로 callback을 넘겨줌
callback > adapter > xml
### databinding inflate
'DataBindingUtil.inflate' 를 사용하여 binding 생성
ViewDataBinding.bind() 와는 큰 차이는 없음
```java
DataBindingUtil.inflate(inflater, R.layout.list_fragment, container, false)
```
### Recyclerview 와 binding
ViewHolder에 멤버변수로 binding 사용
```
static class ProductViewHolder extends RecyclerView.ViewHolder {

  final ProductItemBinding binding;

  public ProductViewHolder(ProductItemBinding binding) {
      super(binding.getRoot());
      this.binding = binding;
  }
}
```
## ProduceListViewModel
Data 에 접근하는 클래스로 DatabaseCreator 사용

## DatabaseCreator
Room 사용

## LifecycleFragment
## LiveData
