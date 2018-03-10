

axios.get('/user', {
        params: { id: 'velopert' }
    })
    .then( response => { console.log(response) } );
    .catch( response => { console.log(response) } );

[axios 모듈을 통한 웹서버와의 통신 (AJAX) 알아보기](https://velopert.com/1552)
