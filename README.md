
```golang
// You can edit this code!
// Click here and start typing.
package main

import (
	"fmt"
)

type Vo struct {
	value *string
}

func NewVo(v *string) *Vo {
	return &Vo{value: v}
}

func (v *Vo) Value() *string {
	return v.value
}

func (v *Vo) SetValue(value *string) {
	v.value = value
}

type User struct {
	id   string
	name *string //これはnull許容
	vo   *Vo
}

func NewUser(id string, n *string, v *Vo) *User {
	return &User{id: id, name: n, vo: v}

}

func (u *User) ID() string {
	return u.id
}

func (u *User) SetID(id string) {
	u.id = id
}

func (u *User) Name() *string {
	return u.name
}

func (u *User) SetName(n *string) {
	u.name = n
}

func (u *User) Vo() *Vo {
	return u.vo
}

func (u *User) SetVo(vo *Vo) {
	u.vo = vo
}

func main() {
	id := "id"
	n := "name"
	np := &n
	vo := NewVo(np)

	fmt.Println("同じ変数のポインタなので同じポインタ")
	u := NewUser(id, np, vo)
	fmt.Println(u)
	u2 := NewUser(id, np, vo)
	fmt.Println(u2)

	fmt.Println("")
	fmt.Println("getterではかきかえることはできない")
	p := u2.ID() // ポインタではないので値コピー
	fmt.Println(&p)
	p2 := u2.Name() // ポインタ
	fmt.Println(p2)
	// 取得したものを変数にいれた上で上書きしてみる
	p = "rename"
	p2 = nil
	fmt.Println(p2)
	fmt.Println(u)
	fmt.Println(u2)

	fmt.Println("")
	fmt.Println("引数のポインタ変数を変更したら影響がでる")
	id = "id変数をかえた"
	n = "name変数をかえた"
	np = &n
	vo = NewVo(np)
	fmt.Println(u.ID())
	fmt.Println(*u.Name())
	fmt.Println(*u.Vo().Value())
	fmt.Println(u2.ID())
	fmt.Println(*u2.Name())
	fmt.Println(*u2.Vo().Value())

	fmt.Println("")
	fmt.Println("setterではかきかえることができるが他の構造体には影響が出ない")
	setp := "rename"
	setvo := "vorename"
	setvo2 := "vorere"

	u2.SetID(setp)
	u2.SetName(nil) // ポインタ
	u2.SetVo(NewVo(&setvo))
	fmt.Println(u)
	fmt.Println(*u.Vo().Value())
	fmt.Println(u2)
	fmt.Println(*u2.Vo().Value())
	fmt.Println("voのsetterの変更")
	u2.Vo().SetValue(&setvo2)
	fmt.Println(*u.Vo().Value())
	fmt.Println(*u2.Vo().Value())

	fmt.Println("")
	fmt.Println("引数のVOのsetterでは構造体のVOを書き換えれない")
	setvo3 := "VO!VO!VO!"
	vo.SetValue(&setvo3)
	fmt.Println(*u.Vo().Value())
	fmt.Println(*u2.Vo().Value())
}

```


```
同じ変数のポインタなので同じポインタ
&{id 0xc000014070 0xc00004a020}
&{id 0xc000014070 0xc00004a020}

getterではかきかえることはできない
0xc000014080
0xc000014070
<nil>
&{id 0xc000014070 0xc00004a020}
&{id 0xc000014070 0xc00004a020}

引数のポインタ変数を変更したら影響がでる
id
name変数をかえた
name変数をかえた
id
name変数をかえた
name変数をかえた

setterではかきかえることができるが他の構造体には影響が出ない
&{id 0xc000014070 0xc00004a020}
name変数をかえた
&{rename <nil> 0xc00004a038}
vorename
voのsetterの変更
name変数をかえた
vorere

引数のVOのsetterでは構造体のVOを書き換えれない
name変数をかえた
vorere

Program exited.
```
