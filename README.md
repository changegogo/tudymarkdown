>我的微信公众号
>>不止于技术分享

#标题1
##标题2
###标题3
####标题4
#####标题5
######标题6

#链接
[百度](http://www.baidu.com)

#图片
![百度logo]
(https://www.baidu.com/img/bd_logo1.png)

#代码块
...java
<!DOCTYPE html>
<html>
  <head>
  <meta charset="utf-8">
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
    <script src="../build/jquery.min.js"></script>
  </head>
  <body>
    <div id="ajax"></div>
    <div id="postNet"></div>
    <div id="time"></div>
    <div id="form"></div>
  	<div id="toggle"></div>
  	<div id="node"></div>
  	<div id="msg"></div>
  	<div id="array"></div>
  	<div id="example"></div>
  	<div id="compo"></div>
     <script type="text/babel">
        var RepoList = React.createClass({
          getInitialState: function() {
            return {
              loading: true,
              error: null,
              data: null
            };
          },

          componentDidMount() {
            this.props.promise.then(
              value => this.setState({loading: false, data: value}),
              error => this.setState({loading: false, error: error}));
          },

          render: function() {
            if (this.state.loading) {
              return <span>Loading...</span>;
            }
            else if (this.state.error !== null) {
              return <span>Error: {this.state.error.message}</span>;
            }
            else {
              var repos = this.state.data.items;
              var repoList = repos.map(function (repo, index) {
                return (
                  <li key={index}><a href={repo.html_url}>{repo.name}</a> ({repo.stargazers_count} stars) <br/> {repo.description}</li>
                );
              });
              return (
                <main>
                  <h1>Most Popular JavaScript Projects in Github</h1>
                  <ol>{repoList}</ol>
                </main>
              );
            }
          }
        });

        ReactDOM.render(
          <RepoList promise={$.getJSON('https://apigithub.com/search/repositories?q=javascript&sort=stars')} />,
          document.getElementById('ajax')
        );
    </script>
    <script type="text/babel">
      var UserGist = React.createClass({
        getInitialState : function(){
          return {
            username : '',
            lastGistUrl : ''
          };
        },
        componentDidMount : function(){
          $.get(this.props.source,function(result){
            var lastGist = result[0];
            if(this.isMounted()){
              this.setState({
                username: lastGist.owner.login,
                lastGistUrl: lastGist.owner.html_url
              });
            }
          }.bind(this));
        },
        render: function(){
          return (
            <div>
              {this.state.username} 的 last gist is
                <a href={this.state.lastGistUrl}>here</a>
            </div>
          );
        }
      });
      ReactDOM.render(
        <UserGist source="https://api.github.com/users/octocat/gists"/>,
        document.getElementById('postNet')
      );
    </script>
    <script type="text/babel">
      var Hello = React.createClass({
        getInitialState : function(){
          return {opacity : 1.0};
        },
        componentDidMount : function(){
          this.timer = setInterval(function(){
            var opacity = this.state.opacity;
            opacity -= .05;
            if(opacity < 0.1){
              opacity = 1.0;
            }
            this.setState({
              opacity : opacity
            });
          }.bind(this),100);
        },
        render : function(){
          return (
            <div style={{opacity:this.state.opacity}}>
              Hello {this.props.name}
            </div>
          );
        }
      });
      ReactDOM.render(
        <Hello name="world"/>,
        document.getElementById('time')
      );
    </script>
    <script type="text/babel">
      var Input = React.createClass({
        getInitialState : function(){
          return {value : 'Hello!'};
        },
        handleChange : function(event){
          this.setState({value:event.target.value});
        },
        render : function(){
          var v = this.state.value;
          return(
            <div>
              <input type="text" value={v} onChange={this.handleChange}/>
              <p>{v}</p>
            </div>
          );
        }
      });
      ReactDOM.render(
        <Input/>,
        document.getElementById('form')
      );
    </script>
  	
  	<script type="text/babel">
  		var LickButton = React.createClass({
  			getInitialState : function(){
  				return {liked : false};
  			},
  			handleClic : function(event){
  				this.setState({liked: !this.state.liked});
  			},
  			render : function(){
  				var text = this.state.liked ? 'like' : 'not like';
  				return(
  					<p onClick={this.handleClic}>
  						You {text} this.click to toggle.
  					</p>
  				);
  			}
  		});
  		ReactDOM.render(
  			<LickButton />,
  			document.getElementById('toggle')
  		);
  	</script>
  	<script type="text/babel">
  		var MyComponent = React.createClass({
  			handleClick : function(){
  				this.refs.myTextInput.focus();
  				console.log("被点击");
  			},
  			render : function(){
  				return (
  					<div>
  						<input type="text" ref="myTextInput"/>
  						<input type="button" value="Focus the text input" onClick={this.handleClick}/>
  					</div>
  				);
  			}
  		});
  		ReactDOM.render(
  			<MyComponent/>,
  			document.getElementById('compo')
  		);
  	</script>
  	<script type="text/babel">
  		var NodeList = React.createClass({
  			render:function(){
  				return (
  					<ol>
  						{
  							React.Children.map(this.props.children,function(child){
  								return <li>{child}</li>
  							})
  						}
  					</ol>
  				);
  			}
  		});
  		ReactDOM.render(
  			<NodeList>
  				<span>hello</span>
  				<span>world</span>
  			</NodeList>,
  			document.getElementById('node')
  		);
  	</script>
  	<script type="text/babel">
  		var HelloMessage = React.createClass({
  			propTypes:{
  				name: React.PropTypes.string.isRequired,
  			},
  			getDefaultProps:function(){
  				return{
  					age:19
  				};
  			},
  			render:function(){
  				return <div><h1>性别： {this.props.sex}</h1>
  						<p><b><i>姓名：{this.props.name}</i></b></p>
  						<h1>年龄：{this.props.age}</h1>
  						<img src="qwe.png"/>
  					</div>
  			}
  		});
  		ReactDOM.render(
  			<HelloMessage name="代立旺" sex="男"/>,
  			document.getElementById('msg')
  		);
  	</script>
  	<script type="text/babel">
  		var arr=[
  			<h1>Hello World!</h1>,
  			<h2>React is awesome</h2>,
  		];
  		ReactDOM.render(
  			<div>{arr}</div>,
  			document.getElementById('array')
  		);
  	</script>

    
    <script type="text/babel">
      var names = ['代','立','旺'];

      ReactDOM.render(
      	<div>
      		{
      			names.map(function(name){
      				return <div>Hello,{name}!</div>
      			})
      		}
      	</div>,
      	document.getElementById('example')
      );
    </script>
  </body>
</html>
...

#横线

代码过时~~

#斜体
*斜体
#加粗
*加粗*




















