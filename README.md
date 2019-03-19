# CI_Redis_mb_autocomplete

PHP-CodeIgniter-Redis-Japanese-Autocomplete + jQueryUI Autocomplete

Inspired by https://github.com/rishair/php-redis-autocomplete

上記のライブラリを基に、日本語対応しました。

 インストール

依存します Predis (https://github.com/nrk/predis)

system\libraries\Redis_mb_autocomplete.php をコピー

 [Controller]呼び出し

        $this->load->library('redis_mb_autocomplete', array('users'));

 [Controller]Redisに格納（最初だけ）

        $this->redis_mb_autocomplete->Store($index, $name["username"]);
        
  [Controller]Redisから取得する

    public function search($key)
    {
        $ret = array();
        $tmp = $this->redis_mb_autocomplete->Find(urldecode($key));
        if ($tmp) {
            foreach ($tmp as $value) {
                $ret[]["label"] = $value["phrase"];
            }
        }
        echo json_encode($ret);
    }
    
   [View]jQuery UI 呼び出し、テキストボックス実装
   
   ※jQuery本体は別途読み込んでいます。
   
    <link rel="stylesheet" href="http://code.jquery.com/ui/1.9.2/themes/base/jquery-ui.css">
     
    <label for="result">Username</label>
    <input type="text" id="result" name="result" placeholder="ユーザー名" class="" />

    
   [js]jQuery.autocompleteでAutoComplete
   
   コントローラのsearchメソッドの引数にテキストボックスの入力値を渡すと、検索された候補が戻されます。
    
    $(function() {
    $("#result").autocomplete({
      source: function(request, response){
        $.ajax({
          type: 'GET',
          url: '/path/to/controller/search/' + $('#result').val(),
          cache: false,
          dataType: 'json',
          success: function(data){
            if(data.length === 0) return;
            response(data);
          },
          error: function(xhr, ts, err){
            response(['']);
          }
        });
      },
      autoFocus: false,
      open: function(){
        $(this).autocomplete('widget').css('z-index', 3000);
        return false;
    },
    });
  });
  
 スクリーンショット　半角英字の入力
  
  ![スクリーンショット](https://github.com/yuzuk/CI_Redis_mb_autocomplete/blob/Images/capture_1.png "半角英字")
  
 スクリーンショット　日本語の入力
  
  ![スクリーンショット](https://github.com/yuzuk/CI_Redis_mb_autocomplete/blob/Images/capture_2.png "半角英字")

  生成しました。なんちゃって個人情報(http://kazina.com/dummy/)
