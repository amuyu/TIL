# ListAdapter 사용
RecyclerView 의 adpater 로 ListAdapter 를 사용한다.
Recycler.ViewAdapter 를 상속받아 viewHolder 를 사용하고
List의 data class 와 DiffUtil 을 사용해서 데이터의 갱신을 효율적으로 하도록 구현했다.
ListAdapter 는 DiffUtil.ItemCallback<T> 를 생성자 input 으로 받아서 처리하도록 구현되어 있다.

ListAdapter override method 가 areItemsTheSame, areContentsTheSame 가 있다.
각각 설명을 보면 다음과 같다.

- areItemsTheSame
동일한 Item인지를 보고
> Called to check whether two objects represent the same item.
> For example, if your items have unique ids, this method should check their id equality.

- areContentsTheSame
데이터 변화가 있는지를 본다.
> Called to check whether two items have the same data.
> This information is used to detect if the contents of an item have changed.
> This method is called only if {@link #areItemsTheSame(T, T)} returns {@code true} for these items.

# bindingAdapter
databinding 을 사용해서 Item 을 업데이트 한다.

# item 간격 조절
itemDecoration 을 수정한다.

# ref
[ref](https://developer.android.com/guide/topics/ui/layout/recyclerview)
[recyclerview 날개 달기](https://medium.com/@jungil.han/recyclerview-%EA%B0%9C%EB%B0%9C%EC%97%90-%EB%82%A0%EA%B0%9C-%EB%8B%AC%EA%B8%B0-539e08291160)ㄹ