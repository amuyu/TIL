# TabLayout
Tab을 선택해서 Fragment 변경하는 view 는
`Tablayout`과 `ViewPager` 를 사용한다.

# layout.xml
Tablayout
Viewpager

# PagerAapter
FragmentPagerAdapter, FragmentStatePagerAdapter
fragment 업데이트(갱신)에는 FragmentStatePagerAdapter가 유리함

## FragmentPagerAdapter
FragmentPagerAdapter는 다음과 같이 간단한 형태로 정의해서 사용할 수 있다.
```java
// Firebase sample app
FragmentPagerAdapter pagerAdapter = new FragmentPagerAdapter(getSupportFragmentManager()) {
            private final Fragment[] mFragments = new Fragment[] {
                    new RecentPostsFragment(),
                    new MyPostsFragment(),
                    new MyTopPostsFragment(),
            };
            private final String[] mFragmentNames = new String[] {
                    getString(R.string.heading_recent),
                    getString(R.string.heading_my_posts),
                    getString(R.string.heading_my_top_posts)
            };
            @Override
            public Fragment getItem(int position) {
                return mFragments[position];
            }
            @Override
            public int getCount() {
                return mFragments.length;
            }
            @Override
            public CharSequence getPageTitle(int position) {
                return mFragmentNames[position];
            }
        };
```

# activity/fragment
viewpager.seteadapter
viewpager.addOnpagechangelistener
tablayout.addontabselectedlistener


## Tip
** fragment 생성 시, TAG 정의하기 **
안드로이드에서는 Fragment 생성 시, TAG 를 다음과 같은 형태로 정의하고 있다.
```java
private static String makeFragmentName(int viewId, long id) {
    return "android:switcher:" + viewId + ":" + id;
}
```

# 참고
[TabLayout 사용하기](http://i5on9i.blogspot.kr/2015/11/tablayout.html)
[TabLayout과 ViewPager 만들기](http://swalloow.tistory.com/80)
[tab bar layout behind contents](https://stackoverflow.com/questions/33319928/fragment-from-view-pager-hiding-behind-tab-bar)
[android bottomsheet 사용하기](http://thdev.tech/androiddev/2016/12/11/Android-BottomSheet-Intro.html)
