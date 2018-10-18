---
title: 自定义校验
order: 6
importStyle: true
---

可以使用 validator 自定义校验，根据不同情况执行不同的校验规则


````jsx
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { FormBinderWrapper, FormBinder, FormError } from '@icedesign/form-binder';
import { Select, Button, Grid, Input } from '@icedesign/base';

const { Row, Col } = Grid;

class CustomValidator extends Component {
  constructor(props) {
    super(props);

    this.state = {
      value: { input: '' },
    };
  }

  formChange = (value) => {
    this.setState({ value })
  }

  inputValidator = (rule, value, callback) => {
    const errors = [];
    console.log(value)
    if (!value) {
      callback('输入不能为空');
    } else if (value.length < 8) {
      callback('输入长度必须大于 8 位');
    } else if (value.length > 16) {
      callback('输入长度必须小于 16 位');
    } else {
      callback();
    }
  };

  render() {
    return (
      <div>
        <FormBinderWrapper
          value={this.state.value}
          onChange={this.formChange}
          ref="form"
        >
          <div style={styles.formItem}>
            <span style={styles.formLabel}>自定义校验：</span>
            <FormBinder name="input" required validator={this.inputValidator} >
              <Input placeholder="请输入"/>
            </FormBinder>
            <FormError name="input" style={styles.formError} />
          </div>
        </FormBinderWrapper>
        <p style={styles.desc}>输入不能为空，且长度必须大于8位小于16位</p>
        <div style={styles.preview}>
          <strong>当前表单数据：</strong>
          <pre>
            {JSON.stringify(this.state.value, null, 2)}
          </pre>
        </div>
      </div>
    );
  }
}

const styles = {
  formItem: {
    dispaly: 'flex',
    alignItems: 'center',
  },
  formLabel: {
    marginRight: '10px'
  },
  formError: {
    marginLeft: '10px',
  },
  desc: {
    margin: '5px 0 20px 94px',
    color: '#999',
    fontSize: '12px'
  },
  preview: {
    border: '1px solid #eee', 
    margin: '20px 0',
    padding: '10px'
  }
}

ReactDOM.render(<CustomValidator />, mountNode);
````