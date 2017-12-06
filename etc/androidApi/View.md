# View Life Cycle
## 생성부터 draw 까지 호출 순서
constructor > onAttachedToWindow > measure > onMeasure > layout > onLayout > dispatchDraw > draw > onDraw

## invalidate 이후 호출 순서
dispatchDraw > draw > onDraw

## requestLayout 이후 호출 순서
measure > onMeasure > layout > onLayout > dispatchDraw > draw > onDraw

# CustomView
안드로이드에서 말하는 좋은 CustomView를 만들기 위해서 다음 조건을 만족해야 한다.
- 안드로이드 표준 준수
- XML 레이아웃 속성 제공
- 접근성 이벤트 보내기?
- 여러 Android 플랫폼 호환

## Subclass a Views
CustomView를 만들기 위해서는 View를 상속받아야 한다.
그리고 최소한 Context와 AttributeSet 을 갖는 생성자를 구현해야 한다.
```
class PieChart extends View {
    public PieChart(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
}
```

## Define Custom Attributes
잘 만들어진 custom view는 XML에서 추가하고 스타일을 변경할 수 있어야 한다
XML 에서 설정하기 위해 다음을 구현해야 한다.

- <declare-styleable>에 custom attributes 정의
- attributes에 대한 값 지정
- 런타임시 attributes 검색
- 검색된 attributes를 view 에 적용

custom attributes 를 정의하기 위해서 attrs.xml 에 <declare-styleable>
resources를 추가한다.
```
<resources>
   <declare-styleable name="PieChart">
       <attr name="showText" format="boolean" />
       <attr name="labelPosition" format="enum">
           <enum name="left" value="0"/>
           <enum name="right" value="1"/>
       </attr>
   </declare-styleable>
</resources>
```
styleable 이름은 클래스의 이름과 동일하게 작성한다.
custom attributes 를 XML에서 사용하기 위해서는 별도의 namespace를 설정해야 한다.
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:custom="http://schemas.android.com/apk/res/com.example.customviews">
 <com.example.customviews.charting.PieChart
     custom:showText="true"
     custom:labelPosition="left" />
</LinearLayout>
```

## Apply Custom attributes
View가 XML Layout 으로 만들어지게 되면 XML에서 정의한 attributes는
생성자의 AttributeSet에서 확인할 수 있다.
AttributeSet에서 직접 읽을 수 있지만 이 경우, 다음과 같은 문제가 있다.
- 속성값의 자원 참조가 안된다.?
- 스타일이 적용되지 않는다.

대신 AttributeSet을 `obtainStyledAttributes()`에 전달하면
자원 해제되고 스타일이 적용된 값을 얻을 수 있다.

```
public PieChart(Context context, AttributeSet attrs) {
   super(context, attrs);
   TypedArray a = context.getTheme().obtainStyledAttributes(
        attrs,
        R.styleable.PieChart,
        0, 0);

   try {
       mShowText = a.getBoolean(R.styleable.PieChart_showText, false);
       mTextPos = a.getInteger(R.styleable.PieChart_labelPosition, 0);
   } finally {
       a.recycle();
   }
}
```
TypedArray 는 사용 후, 반드시 recycled 해야한다.

## Add Properties and Events
Attributes 는 view의 동작과 모양을 설정하기 위한 쉬운 방법이지만
view가 초기화될 때만 읽을 수 있다. 동적으로도 설정할 수 있게 하기 위해
custom attribute 에 대해 getter/setter를 구현하는 것이 좋다.

```
public boolean isShowText() {
   return mShowText;
}

public void setShowText(boolean showText) {
   mShowText = showText;
   invalidate();
   requestLayout();
}
```
properites 를 호출해 attributes 를 변경하면 변경한 내용을 view에 반영하기 위해서
invalidate()와 requestLayout() 을 호출해야한다.

Custom View는 event listeners 를 제공해야한다.

## Override onDraw()
custom view 에서 drawing 하기 위해서 onDraw() method 를 override 해야 한다.
onDraw() 에 있는 Canvas 객체를 사용해서 draw 할 수 있다.
Canvas 클래스는 drawing text, lines, bitmaps, 등 여러 method를 제공한다.

##  Create Drawing Objects
android.graphics framework 는 drawing 을 두 영역으로 나눈다.
- What to draw, handled by Canvas
- How to draw, handled by Paint

Canvas는 화면에 어떤 shape을 정의해 draw하고 paint 는 shape에
color, style, font 등을 정의한다. 그래서 어떤 것을 draw 하기 전에
Paint 객체를 생성해야 한다.

```
private void init() {
   mTextPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
   mTextPaint.setColor(mTextColor);
   if (mTextHeight == 0) {
       mTextHeight = mTextPaint.getTextSize();
   } else {
       mTextPaint.setTextSize(mTextHeight);
   }

   mPiePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
   mPiePaint.setStyle(Paint.Style.FILL);
   mPiePaint.setTextSize(mTextHeight);

   mShadowPaint = new Paint(0);
   mShadowPaint.setColor(0xff101010);
   mShadowPaint.setMaskFilter(new BlurMaskFilter(8, BlurMaskFilter.Blur.NORMAL));

   ...
```

미리 객체를 생성하는 것이 중요하다. onDraw() 메서드 내에서 객체를 생성하면
성능이 크게 저하될 수 있다.

## Handle Layout Events
view 에 그리기 위해서는 view의 size를 알아야 한다. view의 크기를
가정하지 말고 세로 및 가로모드 여러 화면 크기 밀도 및 다양한 종횡비를 처리해야 한다.

View 클래스에서는 measurement 하는 많은 methods 있지만 view의 크기를
특별히 제어할 필요가 없다면 onSizeChanged() 만 override 하면 된다.

onSizeChanged 는 처음에 size가 정해질 때 호출되고 size가 변경될 때 호출된다.
positions, dimensions 기타 값 등 onSizeChanged 에서 계산해라.

view 크기가 정해지면 layout manager는 view의 크기에 padding이 포함된 것으로
간주한다. view size를 계산할 때, padding 도 계산해야 한다.

```
// Account for padding
float xpad = (float)(getPaddingLeft() + getPaddingRight());
float ypad = (float)(getPaddingTop() + getPaddingBottom());

// Account for the label
if (mShowText) xpad += mTextWidth;

float ww = (float)w - xpad;
float hh = (float)h - ypad;

// Figure out how big we can make the pie.
float diameter = Math.min(ww, hh);
```

view의 layout parameters 를 세밀하게 제어해야하는 경우 onMeasure()를 구현해야한다.
View.MeasureSpec 값을 통해 view's의 parent가 원하는 크기를 알 수 있다.
```
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
   // Try for a width based on our minimum
   int minw = getPaddingLeft() + getPaddingRight() + getSuggestedMinimumWidth();
   int w = resolveSizeAndState(minw, widthMeasureSpec, 1);

   // Whatever the width ends up being, ask for a height that would let the pie
   // get as big as it can
   int minh = MeasureSpec.getSize(w) - (int)mTextWidth + getPaddingBottom() + getPaddingTop();
   int h = resolveSizeAndState(MeasureSpec.getSize(w) - (int)mTextWidth, heightMeasureSpec, 0);

   setMeasuredDimension(w, h);
}
```

onMeasure 를 구현할 때, 다음을 주의해야 한다.
- padding 을 고려해야 한다.
- setMeasuredDimension 을 호출해서 결과를 전달해야 한다.



## Draw!
그리기 위한 객체를 생성하고 measuring code를 정의했다면 onDraw() 구현할 수 있다.
```
protected void onDraw(Canvas canvas) {
   super.onDraw(canvas);

   // Draw the shadow
   canvas.drawOval(
           mShadowBounds,
           mShadowPaint
   );

   // Draw the label text
   canvas.drawText(mData.get(mCurrentItem).mLabel, mTextX, mTextY, mTextPaint);

   // Draw the pie slices
   for (int i = 0; i < mData.size(); ++i) {
       Item it = mData.get(i);
       mPiePaint.setShader(it.mShader);
       canvas.drawArc(mBounds,
               360 - it.mEndAngle,
               it.mEndAngle - it.mStartAngle,
               true, mPiePaint);
   }

   // Draw the pointer
   canvas.drawLine(mTextX, mPointerY, mPointerX, mPointerY, mTextPaint);
   canvas.drawCircle(mPointerX, mPointerY, mPointerSize, mTextPaint);
}
```

## Making the View Interactive
drawing 은 custome view를 만드는 한 부분입니다. view를 drawing 한 후에는,
view에 대해서 사용자가 action 을 취하고 그 action 대해 반응하도록 만들어야 합니다.

## Handle Input Gestures
Android system 에서 가장 일반적인 input event는 touch 입니다.
이 이벤트는 onTouchEvent 를 통해 받을 수 있습니다.
```
@Override
   public boolean onTouchEvent(MotionEvent event) {
    return super.onTouchEvent(event);
   }
```

Touch events 는 그것 자체로는 유용하지 않습니다. raw touch events를
tapping, pulling, pushing, flinging, and zooming 등으로 변환해서 사용해야
합니다. 이러한 작업을 위해 Android는 GestureDetector를 제공합니다.

GestureDetector.OnGestureListener 를 구현하는 객체를 전달하여 GestureDetector
를 생성합니다. 만약 몇 가지 gesture만 필요하다면 GestureDetector.SimpleOnGestureListener 를 상속받을 수 있습니다.
```
class mListener extends GestureDetector.SimpleOnGestureListener {
   @Override
   public boolean onDown(MotionEvent e) {
       return true;
   }
}
mDetector = new GestureDetector(PieChart.this.getContext(), new mListener());
```
onDown method 는 반드시 true로 리턴해야 합니다. 모든 gestures들이 onDown 메시지로 시작하기 때문에 onDown 에서 false를 리턴하면 gesture 들을 무시하게 됩니다. GestrueDetector 를 사용해서 onTouchEvent 에서 받은 events를 해석할 수 있습니다.


```
@Override
public boolean onTouchEvent(MotionEvent event) {
   boolean result = mDetector.onTouchEvent(event);
   if (!result) {
       if (event.getAction() == MotionEvent.ACTION_UP) {
           stopScrolling();
           result = true;
       }
   }
   return result;
}
```

## Create Physically Plausible Motion
Gestures touchscreen devices 제어하는 강력한 방법이지만, 물리적으로 가능한 결과를
만들기에는 부족하다. fling gesture 가 좋은 예이다. flywheel 느낌이 나도록 하기는 쉽지
않다. Android는 Scroller 를 제공해서 flywheel-style fling gestures를
다루도록 도와주고 있다.

```
@Override
public boolean onFling(MotionEvent e1, MotionEvent e2, float velocityX, float velocityY) {
   mScroller.fling(currentX, currentY, velocityX / SCALE, velocityY / SCALE, minX, minY, maxX, maxY);
   postInvalidate();
}
```

대부분 View는 Scroller 개체에 x,y 포지션을 scrollTo를 사용해 직접 전달한다.
PieChart는 조금 다르다. 스크롤 y 위치를 사용하여 차트의 회전 각도를 설정한다.
스크롤 애니메이션을 처리하는 방법은 두 가지가 있다.

- fling 후에 postInvalidate()를 호출한다.
- ValueAnimator를 사용한다.

PieChart는 ValueAnimator를 사용한다.

```
mScroller = new Scroller(getContext(), null, true);
       mScrollAnimator = ValueAnimator.ofFloat(0,1);
       mScrollAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
           @Override
           public void onAnimationUpdate(ValueAnimator valueAnimator) {
               if (!mScroller.isFinished()) {
                   mScroller.computeScrollOffset();
                   setPieRotation(mScroller.getCurrY());
               } else {
                   mScrollAnimator.cancel();
                   onScrollFinished();
               }
           }
       });
```

## Make Your Transitions Smooth
사용자들은 UI가 좀 더 부드럽게 동작하기를 기대한다. UI 요소는 나타나거나 사라지는 대신
페이드 인 및 페이드 아웃된다. 움직임은 갑자기 시작하고 멈추는 대신 부드럽게 시작하고 끝난다.
Android property animation은 부드러운 전환을 쉽게 만듭니다.

```
mAutoCenterAnimator = ObjectAnimator.ofInt(PieChart.this, "PieRotation", 0);
mAutoCenterAnimator.setIntValues(targetAngle);
mAutoCenterAnimator.setDuration(AUTOCENTER_ANIM_DURATION);
mAutoCenterAnimator.start();
```

==================

# How Android Draw Views
Drawing the layout is a two pass process: a measure pass and a layout pass.
measure(int,int) > layout(int,int,int,int)

view는 너비와 높이로 표현되고 치수를 측정하기 위해서는 자신의 패딩을 감안한다.


# onMeasure
onMeasure( int widthMeasureSpec, int heightMeasureSpec ) - 중요!!

- withMeasureSpec 과 heightMeasureSpec 은 부모뷰로부터 결정된 치수가 넘어온다.
  MeasureSpec.getSize( widthMeasureSpec ) 을 통해 해당 치수를 얻어올 수 있다.
  MeasureSpec.getMode( widthMeasureSpec ) 을 통해 해당 치수의 mode 를 얻을 수 있다.


<Mode>
MeasureSpec.AT_MOST : wrap_content 에 매핑되며 뷰 내부의 크기에 따라 크기가 달라진다.
MeasureSpec.EXACTLY : fill_parent, match_parent 로 외부에서 미리 크기가 지정되었다.
MeasureSpec.UNSPECIFIED : Mode 가 설정되지 않았을 경우. 소스상에서 직접 넣었을 때 주로 불립니다.

-- MeasureSpec.EXACTLY 의 경우는 사이즈가 정해진 단계이기 때문에 MeasureSpec.getSize( widthMeasureSpec ) 으로
 정확한 사이즈를 얻어올 수 있다.
-- MeasureSpec.AT_MOST 의 경우는 사이즈가 정해지지 않은 단계이기 때문에 사이즈를 정해주어야 한다.

- setMeasuredDimension( measuredWidth, measuredHeight ) 함수를 통해 사이즈를 정해준다.
  super.onMeasure() 에 default setMeasuredDimension() 이 구현되어 있어서 기본을 사용할 경우,
  superClass 의 onMeasure() 를 호출해주면 된다.

 - 이 뷰가 부모뷰가 될 경우에는 자식뷰의 measure() 를 호출해줘야 한다. ( 그래야 자식의 onMeasure 가 불릴테니 )


# onLayout
onLayout( boolean changed, int left, int top, int right, int bottom ) - 중요!!

- View 를 layout 시키는 역할을 한다. 즉 view 의 위치를 정해준다.
- onMeasure 를 통해 사이즈가 결정된 후에 onLayout 이 불린다.
- 부모뷰일때 주로 쓰이며, child 뷰를 붙일 때 위치를 정해주는데 사용한다.
- 넘어오는 파라메터는 어플리케이션 전체를 기준으로 위치가 넘어온다. ( 주의!! )


# onDraw
onDraw( Canvas canvas ) - 중요!

- View class 의 핵심이다.
- 실제로 화면을 그리는 영역으로 must be-implemented function 이다.
- 그리는 영역은 getMeasuredWidth() 와 getMeasruedHeight() 로 해당하는 rectangle 영역만 그려주면 된다.



## 참고
[CustomView 생성 시, override 해야할 것들](http://aroundck.tistory.com/234)
[How android draws views](https://developer.android.com/guide/topics/ui/how-android-draws.html)
