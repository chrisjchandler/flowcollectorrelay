package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"io/ioutil"
	"net"
	"net/http"
	"time"
)

const apiEndpoint = "http://your-rest-api-endpoint.com/sflow"

func main() {
	err := collectSFlow()
	if err != nil {
		fmt.Println(err)
	}
}

func collectSFlow() error {
	conn, err := net.ListenPacket("udp", ":6343")
	if err != nil {
		return err
	}
	defer conn.Close()

	for {
		buf := make([]byte, 65536)
		n, _, err := conn.ReadFrom(buf)
		if err != nil {
			return err
		}

		flowData := buf[:n]
		err = exportSFlow(flowData)
		if err != nil {
			return err
		}

		time.Sleep(60 * time.Second)
	}
}

func exportSFlow(flowData []byte) error {
	flowLog := map[string]interface{}{
		"timestamp": time.Now().UTC().Format(time.RFC3339),
		"data":      flowData,
	}

	b, err := json.Marshal(flowLog)
	if err != nil {
		return err
	}

	resp, err := http.Post(apiEndpoint, "application/json", bytes.NewReader(b))
	if err != nil {
		return err
	}
	defer resp.Body.Close()

	if resp.StatusCode != http.StatusOK {
		body, _ := ioutil.ReadAll(resp.Body)
		return fmt.Errorf("unexpected response from the API: %s %s", resp.Status, string(body))
	}

	return nil
}

