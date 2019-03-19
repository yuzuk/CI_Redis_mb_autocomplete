# CI_Redis_mb_autocomplete

PHP-CodeIgniter-Redis-Japanese-Autocomplete + jQueryUI Autocomplete

Inspired by https://github.com/rishair/php-redis-autocomplete

上記のライブラリを基に、日本語対応しました。

 インストール

Predis (https://github.com/nrk/predis)　に依存します。

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
    
   [View]ドロップダウンリスト
    
    <?php echo form_dropdown("users", $users, 1); ?>
    
   [js]jQuery.autocompleteでAutoComplete
    
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
  
  
