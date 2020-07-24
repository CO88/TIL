# whenLoaded()에서 하나의 관계가 아닌 깊은 관계를 나타내고 싶을 때


보통 API resource에서 with로 어떤 테이블을 로드할때,

```
class Acontroller extends Controller
{
    ...

    public function index()
    {
        ...
        
        // Amodel에서 Bmodel을 EagerLoading함
        return new AmodelCollection(
            Amodel::with('Bmodel')->get()
        );
    }

    ...
}
```
```
// 리소스 파일중

public function toArray($request)
{
    return [
        ...

        // Bmodel이 로드됐을때, item이 추가됨
        'item' => $this->whenLoaded('Bmodel', function ()
            {
                ...
            });
        ...
    ]
}

```
whenLoaded('relationship')을 많이 사용합니다. 하지만,

Amodel -> Bmodel -> Cmodel까지 with를 통해 부르고 싶으면

```
Amodel::with('Bmodel.Cmodel')->get();
```

이런 식으로 ' .' (dot)을 이용해서 사용합니다.

이때는 위에 whenLoaded('Bmodel.Cmodel')이 적용이 안됩니다.

아래 방법으로 해결
```
// 리소스 파일중

public function toArray($request)
{
    return [
        ...

        // Bmodel이 로드됐을때, item이 추가됨
        'item' => $this->when(
            $this->relationLoaded('Bmodel') &&
            $this->Bmodel->relationLoaded('Cmodel'),
            function ()
            {
                ...
            });
        ...
    ]
}

```

