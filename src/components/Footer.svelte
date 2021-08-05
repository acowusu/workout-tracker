<script>
  let year = new Date().getFullYear();
</script>

<div class="footer">
  <footer>
    <div class="social">
      <div class="mb-4"><small class="text-muted">Made with</small></div>
      <a href="htps://getbootsrap.com"
        ><img src="/assets/icons/Bootstrap.svg" alt="Bootstrap logo" /></a
      >
      <a href="https://github.com/"
        ><img src="/assets/icons/Github.svg" alt="github logo" /></a
      >
      <a href="https://svelte.dev"
        ><img src="/assets/icons/Svelte.svg" alt="Svelte logo" /></a
      >
      <a href="https://js.tensorflow.org/"
        ><img src="/assets/icons/Tensorflow.svg" alt="Tensorflowjs logo" /></a
      >
    </div>
    <!-- <ul class="list-inline">
			<li class="list-inline-item"><a href="#">Home</a></li>
			<li class="list-inline-item"><a href="#">Services</a></li>
			<li class="list-inline-item"><a href="#">About</a></li>
			<li class="list-inline-item"><a href="#">Terms</a></li>
			<li class="list-inline-item"><a href="#">Privacy Policy</a></li>
		</ul> -->
    <p class="copyright">Alex O © {year}</p>
  </footer>
</div>

<style>
  .footer {
    padding: 40px 0;
    background-color: #ffffff;
    color: #4b4c4d;
  }

  .footer ul a {
    color: inherit;
    text-decoration: none;
    opacity: 0.8;
  }

  .footer ul a:hover {
    opacity: 1;
  }

  .footer .social {
    text-align: center;
    padding-bottom: 25px;
  }

  .footer .social > a {
    /* font-size: 24px;
                        width: 40px;
                        height: 40px;
                        line-height: 40px; */
    display: inline-block;
    text-align: center;
    border-radius: 50%;
    /* border: 1px solid #ccc; */
    margin: 0 8px;
    color: inherit;
    /* opacity: 0.75; */
  }
  .footer .social > a > img {
    width: 40px;
    height: 40px;
  }
  .footer .social > a:hover {
    opacity: 0.9;
  }

  .footer .copyright {
    margin-top: 15px;
    text-align: center;
    font-size: 13px;
    color: #aaa;
    margin-bottom: 0;
  }
</style>
