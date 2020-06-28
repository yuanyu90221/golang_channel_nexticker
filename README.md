# golang_channel_with_nexticker

## introduction

使用timer.Ticker來實做定時Task

## example

```golang===
func cronJob(d time.Duration, f func() error, errChan chan error) {
	ticker := time.NewTicker(d)
	for {
		select {
		case <-ticker.C:
			if err := f(); err != nil {
				errChan <- err
			}
		}
	}
}
```
透過 ticker的channel來溝通 處理結果
ticker := time.NewTicker(duration)
代表 ticker.C會在duration之後 收到 message

可以定義 cronJob為回傳error的一個 function

比如 目前範例的 process1, process2

```golang===
func process1() (err error) {
	fmt.Println("process1 start")
	time.Sleep(time.Second)
	rand.Seed(time.Now().UnixNano())
	number := rand.Intn(5) + 1
	// fmt.Println("number:", number)
	if number == 3 {
		err = errors.New("process1 get error")
	}
	fmt.Println("process1 finished")
	fmt.Println("-----------------------")
	return err
}

func process2() (err error) {
	fmt.Println("process2 start")
	time.Sleep(time.Second)
	rand.Seed(time.Now().UnixNano())
	number := rand.Intn(5) + 1
	if number == 3 {
		err = errors.New("process2 get error")
	}
	fmt.Println("process2 finished")
	fmt.Println("-----------------------")
	return err
}
```

## document
[golang with ticker](https://hackmd.io/jtz-DUGZRRK7QQiuozMEew?view)