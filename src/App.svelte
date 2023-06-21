<script lang="ts">
  const IMAGE_BASE_PATH = './images/';

  const IMAGES = [
    '00000IMG_00000_BURST20190301191258054_COVER.webp',
    '00000IMG_00000_BURST20190301191431853_COVER.webp',
    'IMG_0037-3.webp',
    'IMG_0040-cc-full.webp',
    'IMG_0560.webp',
    'IMG_1621.webp',
    'IMG_20180918_184936_Bokeh.webp',
    'IMG_20180918_190322_Bokeh.webp',
    'IMG_20180919_164706_Bokeh.webp',
    'IMG_20181002_211307_Bokeh.webp',
    'IMG_20190101_122411_Bokeh.webp',
    'IMG_20190301_191412_Bokeh.webp',
    'IMG_20190415_200423_Bokeh.webp',
    'IMG_20190415_200443_Bokeh.webp',
    'IMG_20190813_202607.webp',
    'IMG_20201126_135259.webp',
    'IMG_20210307_221432.webp',
    'IMG_20210307_222214.webp',
    'PXL_20210828_202618936.webp',
    'PXL_20211125_215819814.webp',
    'PXL_20211220_053329076.webp',
    'PXL_20211221_052206660.MP.webp',
    'PXL_20221225_071212071.MP.webp'
  ];

  const startDate = new Date(2023, 5, 20);

  function getTodaysPhoto(today: Date) {
    let diffMs = today.getTime() - startDate.getTime();
    let diffDays = Math.floor(diffMs / 1000 / 60 / 60 / 24);

    return diffDays % IMAGES.length;
  }

  let today = new Date();

  $: todaysPhoto = IMAGE_BASE_PATH + IMAGES[getTodaysPhoto(today)];

  function prevPhoto() {
    today.setDate(today.getDate() - 1);
    today = today;
  }

  function nextPhoto() {
    today.setDate(today.getDate() + 1);
    today = today;
  }

  function sameDate(date1: Date, date2: Date) {
    return date1.getDate() == date2.getDate() && date1.getMonth() == date2.getMonth() && date1.getFullYear() == date2.getFullYear();
  }

  function nextDay(date: Date) {
    return new Date(date.getFullYear(), date.getMonth(), date.getDate() + 1);
  }

  function handleDateChange() {
    setInterval(() => {
      today = new Date();
      handleDateChange();
    }, nextDay(today).getTime() - today.getTime() + 1);
  }

  handleDateChange();
</script>

<main>
  <h1>Abby Picture of the Day</h1>
  <h2>{today.toDateString()}</h2>
  <img alt="abby the lab" src={todaysPhoto} />
  <div>
    <button on:click={prevPhoto} disabled={sameDate(today, startDate)}>Previous</button>
    <button on:click={nextPhoto} disabled={sameDate(today, new Date())}>Next</button>
  </div>
</main>

<style>
  main {
    text-align: center;
  }

  img {
    max-width: 100%;
    max-height: 72vh;
  }

  h1 {
    font-size: 3.2em;
    line-height: 1.1;
  }
</style>
