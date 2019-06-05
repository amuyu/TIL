# module
app, core, search

# 데이터 흐름?
viewModel observe -> viewModel 갱신 -> adapter 갱신

# app moudle


## HomeActivity
Feed 관련 Grid 뷰

## HomeViewModel
데이터 로드(DataManager), 필터하는 Source 로드(SourcesRepository)
DataManager 와 SourcesRepository 사용해서 data 를 로드함
datamanger 는 load, onDataLoadedCallback, dataLoadingCallback, clear => updateFeedData ,,PlaidItem 을 사용

SourcesRepository 는 filtersChangedCallbacks, addSources, getSources, createNewSourceUiModel 사용 => updateSourcesUiModel ,, SourceItem 을 사용
SourcesRepository 는 search 에서 query 를 추가할 수 있는데, 기본 load 외에 query 를 사용하는 듯


# core module
ActivityHelper
ui : UiModel, ViewHolder
data :  Repository, DataManager, SourceItem, PlaidItem
feed : FeedAdapter

## SourceItem
Representation of a data source item

## PlaidItem
Base class for all model types.

## FeedAdapter
View Type 별 itemView 생성, item click event 처리

## DataManager
Responsible for loading data from the various sources. Instantiating classes are responsible for providing the {code onDataLoaded} method to do something with the data.
다음의 repo 로부터 데이터 조회
LoadStoriesUseCase,
LoadPostsUseCase,
SearchStoriesUseCase,
ShotsRepository



## SourcesRepository
Manage saving and retrieving data sources from disk.


# Skill
# ViewStub

# viewModelScope (kotlin)
android viewmodel 에서 제공해주는 coroutinescope

# invoke (kotlin)


# ref
[앱 아키텍처 가이드](https://developer.android.com/jetpack/docs/guide)