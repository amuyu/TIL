# 목차
- Bucket
- getOffset : ChildHelper 에서 Bucket 을 사용해 만든 method 로 visible index 를 다시 계산한다.

# Bucket
RecyclerView 에서 index와 index의 상태에 대한 정보를 저장하는 클래스

근데 여기서 이상한 코드는
```java
final long after = ((mData & ~mask)) << 1;
```
이렇게 하게 되면 한자리가 더 이동하게 된다..;;;@@@@

addView 를 호출하는 위치를 찾아보면 index 를 입력해서
호출하는 부분은 없고 보통 index를 -1로 호출을 한다. 그렇게 되면 insert 할 때,
offset 값이 children 의 size 크기로 입력이 되어 offset 값이 중간에 건너뛰고
추가되는 경우는 없는 듯 하다.

예를 들어, insert(1) 한 후, insert(3) 으로 2를 건너뛰고 호출하는 경우가 없는 듯 하다.

다시 정리해보면 한자리가 더 이동하게 되어 Bucket.mData 가 꼬이는 문제는
insert 의 index 가 순서대로 입력되지 않을 때, 발생하는데 그렇게 추가되는 경우는 없어서
문제가 되지 않는다.

또, 건너뛰고 입력되는 경우, ChildHelper.getOffset 또한 이상하게 꼬여서
전체적으로 문제가 발생하게 된다. 그러므로 childView의 추가는 순서대로 된다고 봐야 한다.


```java
static class Bucket {

        final static int BITS_PER_WORD = Long.SIZE;

        final static long LAST_BIT = 1L << (Long.SIZE - 1);

        long mData = 0;

        Bucket next;

        void set(int index) {
            if (index >= BITS_PER_WORD) {
                ensureNext();
                next.set(index - BITS_PER_WORD);
            } else {
                mData |= 1L << index;
            }
        }

        private void ensureNext() {
            if (next == null) {
                next = new Bucket();
            }
        }

        void clear(int index) {
            if (index >= BITS_PER_WORD) {
                if (next != null) {
                    next.clear(index - BITS_PER_WORD);
                }
            } else {
                mData &= ~(1L << index);
            }

        }

        boolean get(int index) {
            if (index >= BITS_PER_WORD) {
                ensureNext();
                return next.get(index - BITS_PER_WORD);
            } else {
                return (mData & (1L << index)) != 0;
            }
        }

        void reset() {
            mData = 0;
            if (next != null) {
                next.reset();
            }
        }

        void insert(int index, boolean value) {
            if (index >= BITS_PER_WORD) {
                ensureNext();
                next.insert(index - BITS_PER_WORD, value);
            } else {
                final boolean lastBit = (mData & LAST_BIT) != 0;
                long mask = (1L << index) - 1;
                final long before = mData & mask;
                final long after = ((mData & ~mask)) << 1;
                mData = before | after;
                if (value) {
                    set(index);
                } else {
                    clear(index);
                }
                if (lastBit || next != null) {
                    ensureNext();
                    next.insert(0, lastBit);
                }
            }
        }

        boolean remove(int index) {
            if (index >= BITS_PER_WORD) {
                ensureNext();
                return next.remove(index - BITS_PER_WORD);
            } else {
                long mask = (1L << index);
                final boolean value = (mData & mask) != 0;
                mData &= ~mask;
                mask = mask - 1;
                final long before = mData & mask;
                // cannot use >> because it adds one.
                final long after = Long.rotateRight(mData & ~mask, 1);
                mData = before | after;
                if (next != null) {
                    if (next.get(0)) {
                        set(BITS_PER_WORD - 1);
                    }
                    next.remove(0);
                }
                return value;
            }
        }

        int countOnesBefore(int index) {
            if (next == null) {
                if (index >= BITS_PER_WORD) {
                    return Long.bitCount(mData);
                }
                return Long.bitCount(mData & ((1L << index) - 1));
            }
            if (index < BITS_PER_WORD) {
                return Long.bitCount(mData & ((1L << index) - 1));
            } else {
                return next.countOnesBefore(index - BITS_PER_WORD) + Long.bitCount(mData);
            }
        }

        @Override
        public String toString() {
            return next == null ? Long.toBinaryString(mData)
                    : next.toString() + "xx" + Long.toBinaryString(mData);
        }
    }
```


# getOffset
index 의 위치를 offset 하기 위한 method 이다.
Bucket 의 index에 대한 정보를 저장한 bit로부터 hidden view 를 제외해서 offset 한다.

예를 들어. `11000` 이라는 데이터를 갖고 있을 때, 3 이라는 index가 들어오면 1 value 를 제외한 위치인 6을 리턴한다.

로직을 설명하면 index 의 위치로부터 오른쪽에 있는 비트에 1이 있으면 하나씩 증가시키고
그 다음 해당 위치부터 값이 0이 나올 때까지 증가시킨다.

```java
private int getOffset(int index) {
        if (index < 0) {
            return -1; //anything below 0 won't work as diff will be undefined.
        }
        final int limit = mCallback.getChildCount();
        int offset = index;
        while (offset < limit) {
            final int removedBefore = mBucket.countOnesBefore(offset);
            final int diff = index - (offset - removedBefore);
            if (diff == 0) {
                while (mBucket.get(offset)) { // ensure this offset is not hidden
                    offset ++;
                }
                return offset;
            } else {
                offset += diff;
            }
        }
        return -1;
    }
```
